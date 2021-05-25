---
title: "Regularization Techniques in Regression Models"
date: 2021-05-16
draft: false
---
What is *regularization* in regression models? Regularization is a modification to a learning algorithm that aims to reduce its generalization error by not its training error. This technique is often used for very high-dimensional data in which the number of features (or predictors, explanatory variables) exceeds the sample sample size. 

## Prerequisites and Context
Our context for this post is linear regression where the target variable $y$ is explained by $n$ features $X = (x_1, x_2, \ldots, x_n)$, with corresponding coefficients $\beta = (\beta_1, \beta_2, \ldots, \beta_n)$. Recall the cost function (with respect to $\beta$) of the linear regression algorithm is given by $\Vert y - X\beta \Vert_2^2$ over $N$ data points. Regression by OLS then estimates the coefficients $\beta$ (and the intercept) by minimizing the squared prediction errors across the training data. To increase model accuracy, one tries to fit as many datapoints by moving the model from linear to quadratic to polynomial. However, as model complexity grows, higher degree model and large coefficients increase the variance significantly leading to overfitting.

In order to avoid this overfitting scenario and increase the model robustness, we can do two things: (1) shrink the coefficients in the model and (2) get rid of high degree polynomials features in the model. This is precisely known as *regularization*. 

However, by shrinking the coefficients of an already optimized $\beta$, we lose model accuracy. To balance the accuracy levels, we add Bias ... part of the model that is not dependent on feature data. By adding Bias in the model, we consequently reduce the variance. In other words, as input variables are changed, the model's prediction changes less than it would have without the bias. 

Ridge and Lasso regularization work by adding a new term to the cost function used to derive the regression formula. 

## Ridge and Lasso Regression
Ridge Regression finds coefficients by minimizing the sum of squared error loss, subject to $L_2$ norm constraint on the coefficients. That is, 
$$\hat{\beta}(\lambda) = \operatorname*{argmin}_\beta \Vert y - X\beta \Vert_2^2 + \lambda \Vert\beta\Vert_2^2$$
where $X$ is the matrix of features (or variables), $y$ is a response vector, $\beta$ is the coefficient vector, and $\lambda$ is a **positive regularization parameter** or a tuning parameter. (Note that $\lambda$ is a hyperparameter, otherwise the gradient descent method would simply set it to zero). As the coefficients $\beta$ vary in the minimization process, each particular combination value of $\beta$ will generate a bias output . The size of the output depends on the total magnitude of all the coefficients and including extra features will result in increased values. 

Sometimes geometry helps. Consider an example with two coefficients, say $\beta = (\beta_1, \beta_2)$ and lets suppose $\lambda = 1$. There can be multiple values that generate the same bias such as $(1, 0), (-1, 0), (0, -1)$. In the $L_2$ norm, the various combinations of $\beta_1$ and $\beta_2$ that generate the same bias form a circle.

![l2 norm](/images/ridge_l2_norm.png)

In this figure, the black circle are the constraint regions $\beta_1^2 + \beta_2^2 < t$ for some $t$ and the colored circles are the contours of the errors from the Gradient Descent. The additional bias term wants to pull the values of $\beta_1, \beta_2$ somewhere on black circle, while gradient descent is trying to travel to the global minimum represented by the dot. This balancing act eventually settles on the intersection indicated by the red cross. 

The Lasso Regression is similar, except it penalizes the size of the $L_1$ norm of the cofficients, that is 
$$\hat{\beta}(\lambda) = \operatorname*{argmin}_\beta \Vert y - X\beta \Vert_2^2 + \lambda \Vert\beta\Vert_1$$ where, again, $\lambda$ is a positive tuning parameter. From a geometric view, we have 

![l1 norm](/images/lasso_l1_norm.png)

The fundamental difference between Ridge and Lasso is that the $L_1$ norm constraint yields a [sparse solution](https://stats.stackexchange.com/questions/45643/why-l1-norm-for-sparse-models). In other words, the point of intersection between $L_1$ norm and the gradient descent contour to converge near the axes, and thus assigning the coefficient value to zero. By assigning zero coefficients to a subset of variables, the Lasso provices automatic feature selection, where the regularization parameter $\lambda$ controls the feature selection. 


## Code Sample 
Here is sample code to run Lasso Regression in Julia, using the package `GLMNet`. 

```julia
# replicates the R-GLM analysis from https://bmcmedresmethodol.biomedcentral.com/articles/10.1186/s12874-019-0681-4
using Flux
using Gnuplot
using GLMNet
using CSV
using DataFrames
using PrettyTables
using Random
using LinearAlgebra

_data = DataFrame(CSV.File("regularized_glm_data.csv")) 
filter!(r -> r.bare_nuclei != "NA", _data) 
_data.bare_nuclei = parse.(Int64, _data.bare_nuclei)
_data |> pretty_table

# sampling the test/training set 
index = 1:nrow(_data) 
testindex = rand(index, Int64(trunc(length(index)/3)))
testdf = _data[testindex, :]
traindf = _data[Not(testindex), :] 
println("testdf: $(nrow(testdf)) traindf: $(nrow(traindf))")

# using the training data, set up vectors for glmnet
y = map(x -> x == 2 ? 0 : 1, traindf[:, :class]) # convert outcmoe variable to 0/1
X = Matrix(traindf[:, 3:11])
path = glmnet(X, y)

path.a0 # intercept values
path.betas # coefficient values

# for each solution (i.e. for each lambda value), sum up the coefficient values for all 9 predictors 
# why plot against the maximum?
betaNorm = [norm(x, 1) for x in eachslice(path.betas,dims=2)] # norm(_, 1) is sum

#@gp "reset session"
@gp "reset"
@gp :- "set xlabel 'maximum beta (coefficient) for each predictor'"
@gp :- "set ylabel 'beta value over each parameter value'"
for p in 1:9 
@gp :- betaNorm path.betas'[:, p] "title 'var: $p' with lines" :-
end
@gp

# To predict the output for each model along the path for a given set of predictors
yt = map(x -> x == 2 ? 0 : 1, testdf[:, :class]) # convert outcmoe variable to 0/1
Xt = Matrix(testdf[:, 3:11])
predict(path, Xt)

# here each row represents the final output of the regression over all the lambda values 
# for row[1] with 56 columns means the output of y = ax1 + ax2 + ... where each value is a continous version of y 
# and we have 56 such guesses over the 56 lambda values 
# which lambda value is best? (i.e. which row to pick)

# use the training set to do n-fold cross-validation. 
cv = glmnetcv(X, y)
minloss_idx = argmin(cv.meanloss)
coef(cv) # equivalent to cv.path.betas[:, minloss_idx]

# now we know what lambda is best, pick that lambda and see the prediction accuracy on the testset
yht = round.(predict(path, Xt, outtype = :prob), digits=3);
yht[:, minloss_idx] # get the prediction from the lambda parameter that is minimized

# or alternatively, just predict using the cross fold result... they are equivalent. 
yht = round.(predict(cv, Xt, outtype = :prob), digits=4)

# Compare against the actual y values 
DataFrame(target=yt, predict=yht) |> pretty_table

```

## Other norms?
While regularization is often with $L_1$ or $L_2$ norms, it could be done with other norms as well. For example, one may use $L_0$  which which simply counts the number of nonzero components of a vector (but actually isn't strictly a norm in the mathematical sense). However, this is not common. See answers and comments on [this](https://stats.stackexchange.com/questions/269298/why-do-we-only-see-l-1-and-l-2-regularization-but-not-other-norms/269407)


### Reference
The Elements of Statistical Learning by Trevor Hastie, Rob Tibshirani, Jerome Friedman. Link: https://web.stanford.edu/~hastie/ElemStatLearn/

