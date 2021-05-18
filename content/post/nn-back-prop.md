---
title: "A brief introduction to Artificial Neural Networks"
date: 2021-05-09
draft: false
---
I wrote this brief article to describe the basics of an artificial neural network.

Consider a neural network with $L$ layers, defined by a mapping $\mathbb{R}^{n_1} \rightarrow \mathbb{R}^{n_L}$ where $n_1$ and $n_L$ are the dimensions of the initial input and final output. Let $W^{[l]} \in \mathbb{R}^{n_l \times n_{l - 1}}$ denote the matrix of weights at layer $l$. Similarly, let $b^{[l]} \in \mathbb{R}^{n_l}$ be the vector of biases for layer $l$.  Given an input $x \in \mathbb{R}^{n_1}$, the neural network can then be succinctly described by   
$$
\begin{align}
    a^{[1]} &= x \in \mathbb{R}^{n_1} \newline
    a^{[l]} &= \sigma \left( W^{[l]} a^{[l - 1]}  +  b^{[l]} \right) \quad \text{ for } l = 2, 3, \ldots, L
\end{align}
$$
where $\sigma$ is some activation function. The expression $W^{[l]} a^{[l - 1]}  +  b^{[l]} \in \mathbb{R}^{n_l}$ is the *weighted input* to evaluate the network at layer $l$, ultimately feeding  data forward through the network to produce a final output $a^{[L]} \in \mathbb{R}^{n_L}$. The values of $W$ and $b$ are estimated by training the neural network, often involving optimizing or minimizing the quadratic function: 
$$ 
C(p) = \frac{1}{N} \sum_{i = 1}^{N} \lVert y(x^{i}) - a^{[L]}(x^{i})  \rVert^2_2
$$
where $\\{ y(x^{i}) \\} _ {i = 1}^{N} \in \mathbb{R}^{n_L}$ are target outputs for $N$ given inputs $\\{ x^{i} \\} _ {i = 1}^{N}$ and $p \in \mathbb{R}^{s}$ is a $s$-dimensional vector of parameters containing the weights and biases. The function $C$ is a linear combination of individual terms that runs over the training data and is referred to as the cost function. Note its
dependence on $p$ rather than data. The classic method for optimization is the steepest descent method, an iterative procedure that computes a sequence of vectors in $\mathbb{R}^{s}$ with the aim of converging to a vector that minimizes the cost function. Simply speaking, given a current vector $p$, choose a perturbation $\Delta p$ so that the $C(p + \Delta p)$ represents an improvement. Taking the Taylor series, and ignoring higher order terms, we have 
$$
C(p + \Delta p) \approx C(p) + \nabla C(p)^{T} \Delta p
$$
where $\nabla C(p)$ is the [gradient](https://en.wikipedia.org/wiki/Gradient) of $C$ w.r.t to $p$. Similar to $C$, the gradient $\nabla C(p)$ is a linear combination of individual terms that runs over the training data. To minimize $C$, we choose $\Delta p$ to make $\nabla C(p)^{T} \Delta p$ as negative as possible. Applying the Cauchy-Schwarz inequality (which states $f, g \in \mathbb{R}^s$, we have $|f^Tg| \leq \lVert f \rVert_2 \lVert g \rVert_2$ so the most negative that $f^Tg$ can be is $\lVert f \rVert_2 \lVert g \rVert_2$, which happens when $f = -g$), we choose $\Delta p$ to lie in the direct of $-\nabla C(p)$. This leads to the update $p \rightarrow p - \eta \nabla C(p)$ where $\eta$ is a small step size, also known as the learning rate. The sequence of vectors is continously updated until some stopping criterion is met. This defines the steepest descent method. 

In general, computing the cost function (or more precisely, the gradient vector within) is prohibitively expensive. Indeed, it involves summing individual terms that run over the training data since,  
$$ 
\nabla C(p) = \frac{1}{N} \sum_{i = 1}^N  \nabla \lVert y(x^{\{i\}})-a^{[L]}(x^{\{i\}})\rVert_{2}^{2}
$$ 
where the parameter for the derivative $p$ arises only implicitly through the neural network computation layers $a^{[l]}, l = \\{1, 2, \ldots L\\}$. When there are a large number of parameters and a large number of training points, computing this gradient vector at every iteration of the steepest descent method is not feasible. Cheaper alternative exists such as replacing the mean of the individual gradients by the gradient at a single, randomly chosen training point, that is: $\nabla C(p) = \frac{1}{N}  \nabla \lVert y(x^{\{j\}})-a^{[L]}(x^{\{j\}})\rVert_{2}^{2}$ where $j$ is some fixed integer sampled uniformly from $\\{1, 2, 3, \ldots, N\\}$. This is also known as the stochastic gradient method. Ofcourse, now that the mean was traded for a single sample, the update $p \rightarrow p - \eta \nabla C(p)$ is not gauranteed to reduce the overall cost. This can be remedied by other modified methods. For example, the index $j$ can be chosen by sampling with replacement -- after using a training point, it is returned to the training set and is equally likely to be picked again for the next cost computation. Batching techniques also exist, where instead of taking the mean over all data points, we consider a smaller sample $m \ll N$.  


We are now in a position to apply the stochastic gradient method in order to train an artificial neural network. Our task is to compute partial derivatives of the cost function $C$ with respect to the parameter vector $p$, or more specfically to each of $w_{j,k}^{[l]}$ and $b_j^{[l]}$. The algorithm used to compute these partial derivatives (the gradient) with respect to a cost function is also known as **Back Propagation**, described in more detail in a follow up blog post to be published later.
