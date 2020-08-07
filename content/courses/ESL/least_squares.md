---
title: 'Linear Methods for Regression'
linktitle: '1: Least Squares'
menu:
  esl:
    parent: Chapter 3
    weight: 1
type: docs
date: "2020-07-24T00:00:00Z"
---

## Least Squares

$$f(X) = \beta_0 + \sum_{j=1}^p X_j\beta_j$$

- The linear model either assumes that the regression function $E[Y|X]$ is linear, or that the linear model is a reasonable approximation

- Training data $(x_1,y_1),\dots (x_N,y_N)$. Each $x_i = (x_{i1}, \dots x_{ip})$ is the feature measurment for the $i$th case. Least Squares criterion, pick coefficient $\beta_0,\dots \beta_j$ by minimizing

$$RSS(\beta) = \sum_{i=1}^N (y_i - f(x_i))^2 = \sum_{i=1}^N (y_i - \beta_0 - \sum_{j=1}^p x_{ij}\beta_j))^2$$

- :question: From a statistical point of view, this criterion is reasonable if the training data $(x_1,y_1),\dots (x_N,y_N)$ represent independent draws from their population. Even if the $x_i$'s are not drawed randomly, the criterion is still valid if the $y_i$'s are conditionally independent given the input $x_i$.

- It might happen that the columns of $\textbf{X}$ are not linearly independent. Then $\hat\beta$ is not uniquely defined, however, $\hat y = \textbf{X}\hat\beta$ is still the projection of $\textbf{y}$ onto the column space of $\textbf{X}$;
there is just more than one way to express that projection in terms of the columns of $\textbf{X}$. Most regression software packages detect these redundancies and automatically implement some stragety for removing them.

## Gauss-Markov Theorem

The least square estimates of $\beta$ has the smallest variance among all linear unbiased estimates. We focus on estimation of any linear
combinations of the parameters $\theta = a^T\beta$. Gauss-Markov states that for any $\tilde\theta = c^Ty$ that is unbiased for $\theta=a^T\beta$, then

$$Var(a^T\hat\beta_{ols}) \le Var(c^Ty)$$

- Consider $MSE(\theta, \tilde\theta) = E(\theta - \tilde\theta)^2 = Var(\tilde\theta) + E(\theta - E[\tilde\theta])^2$. Gauss Markov states that the least square estimate has the smallest MSE among all linear unbiased estimators.

- Biased estimates are commonly used. Trade a little bias for a large reduction in variance.

- Any methods that shrink or sets to zero some of the least squares coefficients may result in a biased estimate. From a more pragmatic point of view, most models are distortions of the truth, and hence are biased; picking the right model amounts to creating the balance between bias and variance.

- Expected prediction error and mean squared error differ only by the constant $\sigma^2$.


## Mutiple Regression from Univariate Regression

- Suppose the inputs $x_1, \dots x_p$ (the columns of the data matrix $\textbf X$) are orthogonal, that is $<x_i, x_j> = 0$. Then the multiple least squares estimates $\hat\beta_j = <x_j, y> / <x_j, x_j>$.

- When the inputs are orthogonal, they have no effect on each other's parameter estimates in the model.

- Orthogonal inputs often come with balanced, designed experiments but almost never with observational data.

- Gram-Schmidt procedure: $x_1 \dots x_p$ --> $z_1 \dots z_p$, then

$$\hat y = \sum_{i=1}^p \hat\beta_p z_p = \sum_{i=1}^p \frac{<z_i, y>}{<z_i, z_i>}z_i$$

- Note that here $\hat\beta_j$ are not least square estimates. In fact, only the last one $\hat\beta_p$ equals the least squares estimate.

- Therefore, by rearranging the $x_j$ any one can be in the last position, and we have shown that the $j$th multiple regression coefficient is the univariate regression coffcient of $y$ on $x_{j012...(j-1)(j+1)...p}$, the residual after regressing $x_j$ on $x_0, x_1\dots x_{j-1}, x_{j+1}, \dots x_p$.

- The multiple regression coefficient $\hat\beta_j$ represents the additional contribution of $x_j$ on $y$, after $x_j$ has been adjusted for $x_0, x_1, \dots x_{j-1}, x_{j+1}, \dots x_p$.

- :question: Relationship with the add-variable plot.

- If $x_p$ is highly correlated with some of the other $x_k$'s, the residual vector $z_p$ will be close to zero, and hence $\beta_p$ will be unstable. This will be true for all the variables in the correlated set. In such situations we might have all the Z-scores be smaller, any one can be deleted yet we cannot delete all of them. The precision with which we can estimate $\beta_p$ depends on how much of $x_p$ is unexplained by the other $x_k$'s.

$$Var(\hat\beta_p) = \frac{\sigma^2}{<z_p, z_p>} = \frac{\sigma^2}{||z_p||^2}$$

- QR decomposition.