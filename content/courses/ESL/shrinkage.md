---
title: 'Shrinkage Methods'
linktitle: '3: Shrinkage Methods'
menu:
  esl:
    parent: Chapter 3
    weight: 3
type: docs
date: "2020-08-03T00:00:00Z"
---

Subset selection produces a model that is interpretable. However, because it is a discrete process, it often exhibits high variance, and so doesn't reduce the prediction error of the full model. Shrinkage methods are more continuous, and don't suffer much from high variability.

## Ridge Regression

- The idea of penalizing by the sum-of-squares of the parameters is also used in neural networks, where it is known as *weight decay*.

- When there are many correlated variables in a linear regression model, their coefficients are poorly determined and exhibit high variance. By imposing a constraint on the size of the parameters this problem is alleivated.

- After reparametrization using $x_{ij}$ --> $x_{ij} - \bar x_j$, we estimate $\beta_0 = \bar y$ and the remaining coefficients by a ridge regression without intercept, using the centered $x_{ij}$.

$$\hat\beta_{ridge} = (X^TX+\lambda I)^{-1}X^Ty$$

- In the case of orthogonormal inputs, $\hat\beta_{ridge} = \frac{1}{1+\lambda}\hat\beta$

- SVD decomposition $X = UDV^T$, then

$$X\hat\beta_{ls} = UU^Ty = \sum_{j=1}^p u_ju_j^Ty$$

$$X\hat\beta_{ridge} = \sum_{j=1}^p u_j\frac{d_j^2}{d_j^2+\lambda}u_j^Ty$$

- Note that $U^Ty$ is the coordinates of $y$ with repect to the orthonormal basis $U$. Like linear regression, ridge regression computes coordinates of $y$ with repect to the orthonormal basis $U$, then shrinks these coordinates by the factors $d_j^2/(d_j^2+\lambda)$.

- Eigen decomposition $X^TX = VD^2V^T$. The eigenvectors $v_j$ are called the principal components directions of $X$.The first principal component direction $v_1$ has the property that $z_1 = Xv_1$ has the largest sample variance $d_1^2/N$ among all normalized linear combinations of columns of $X$. Subsequent $z_j=Xv_j$ have maximum variance $d_j^2/N$, subject to being orthgonal to earlier ones. In fact, $z_j = Xv_j = u_jd_j$ and the derived variables $z_j$ are called principal components of $X$.

- Hence, smallest singular value $d_j$ corresponds to directions in the column space of $X$ having small variance, and ridge regression shrinks these directions of the most. There is an implicit assumption in ridge regression that response tends to vary most in the directions of high variance inputs.

- Effective degree of freedom:

$$df(\lambda) = tr(X(X^TX+\lambda I)^{-1}X^T) = \sum_{j=1}^p \frac{d_j^2}{d_j^2+\lambda}$$


## The Lasso and Comparisons

In the case of orthonormal inputs:

$$\hat\beta^{subset}_j = \hat\beta_j I(|\hat\beta_j| \ge |\hat\beta_M|)$$

$$\hat\beta^{ridge}_j = \hat\beta_j / (1+\lambda)$$

$$\hat\beta^{lasso}_j = \textrm{sign}(\hat\beta_j)(|\hat\beta_j| - \lambda)_+$$
