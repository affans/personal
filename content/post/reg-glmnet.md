---
title: "Regularization in Machine Learning"
date: 2021-05-16
draft: false
---
What is *regularization* in machine learning algorithms? Regularization is a modification to a learning algorithm that aims to reduce its generalization error by not its training error. This technique is often used for very high-dimensional data in which the number of features (or predictors, explanatory variables) exceeds the sample sample size. 

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
$$\hat{\beta}(\lambda) = \operatorname*{argmin}_\beta \Vert y - X\beta \Vert_2^2 + \lambda \Vert\beta\Vert_1$$ where, again, $\lambda$ is a positive tuning parameter. The fundamental difference between Ridge and Lasso is that the $L_1$ norm constraint yields a sparse solution. By assigning zero coefficients to a subset of variables, the Lasso provices automatic feature selection, where the regularization parameter $\lambda$ controls the feature selection. 

From a geometric view, we have 

![l1 norm](/images/lasso_l1_norm.png)


Why do L1 norms achieve sparse solutions? See this Math.ME post (https://stats.stackexchange.com/questions/45643/why-l1-norm-for-sparse-models).



Stable and accurate regression/classification model will require some sort of penalization on the L1 or the L2 norm of the coefficients. 
This L1/L2 "regularization" brings in many technical advantages. 

 
 The simplest non-mathematical explanation is that For L2: Penalty term is squared,so squaring a small value will make it smaller. We don't have to make it zero to achieve our aim to get minimum square error, we will get it before that. For L1: Penalty term is absolute,we might need to go to zero as there are no catalyst to make small smaller.



Underfitting = High Bias
Overfitting = High Variance
Higher degree model and large coefficients increase the variance significantly leading to Overfitting
