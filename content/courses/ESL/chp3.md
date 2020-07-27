---
title: 'Linear Methods for Regression'
linktitle: 'Notes'
menu:
  esl:
    parent: Chapter 3
    weight: 1
type: docs
date: "2020-07-24T00:00:00Z"
---

## Chapter 3.2

$$f(X) = \beta_0 + \sum_{j=1}^p X_j\beta_j$$

- The linear model either assumes that the regression function $E[Y|X]$ is linear, or that the linear model is a reasonable approximation.

- Training data $(x_1,y_1),\dots (x_N,y_N)$. Each $x_i = (x_{i1}, \dots x_{ip})$ is the feature measurment for the $i$th case.

- Least Squares criterion, pick coefficient $\beta_0,\dots \beta_j$ by minimizing

$$RSS(\beta) = \sum_{i=1}^N (y_i - f(x_i))^2 = \sum_{i=1}^N (y_i - \beta_0 - \sum_{j=1}^p x_{ij}\beta_j))^2$$

- [?] From a statistical point of view, this criterion is reasonable if the training data $(x_1,y_1),\dots (x_N,y_N)$ represent independent draws from their population. Even if the $x_i$'s are not drawed randomly, the criterion is still valid if the $y_i$'s are conditionally independent given the input $x_i$.

- Assuming that $\textbf{X}$ has full column rank, hence that $\textbf{X}^T\textbf{X}$ is positive definite, the unique solution is

$$\hat\beta = (\textbf{X}^T\textbf{X})^{-1}\textbf{X}^T\textbf{y}$$

- It might happen that the columns of $\textbf{X}$ are not linearly independent. Then $\hat\beta$ is not uniquely defined, however, $\hat y = \textbf{X}\hat\beta$ is still the projection of $\textbf{y}$ onto the column space of $\textbf{X}$;
there is just more than one way to express that projection in terms of the columns of $\textbf{X}$. Most regression software packages detect these redundancies and automatically implement some stragety for removing them.

