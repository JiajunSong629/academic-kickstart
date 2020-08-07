---
title: 'One-dimensional Kernel Smoothers'
linktitle: '1: One-dimensional Kernel Smoothing'
menu:
  esl:
    parent: Chapter 6
    weight: 1
type: docs
date: "2020-08-05T00:00:00Z"
---

## Kernel Smoothing Methods

- Estimate the regression function $f(X)$ over the domin $R^p$ by fitting separately a different model at each query point $x_0$.

- This is done by only using those observations close to the target point $x_0$ to fit the simple model, and in such way the resulting estimated function $\hat f(X)$ is smooth in $R^p$.

## One-dimensional Kernel Smoothers

- This localization is done via a weighting function or kernel $K_\lambda(x_0, x_i)$.

- Memory-based method. Requires in principle little or no training. The only parameter needs to be determined from the training set is $\lambda$.

- K-nearest-neighbor average is discrete and leads to discontinuous $\hat f(x)$.

- Assign weights that die smoothly.

$$\hat f(x_0) = \frac{\sum_{i=1}^N K_\lambda (x_0, x_i)y_i}{\sum_{i=1}^N K_\lambda (x_0, x_i)}$$

$$K_\lambda(x_0, x) = D(\frac{|x - x_0|}{\lambda})$$

- The size of k-nearest-neighbor adapts to the local density of $x_0$. We could more generally have $h_\lambda(x_0)$ be the width function of the neighborhoods.

- Large $\lambda$ implies lower variance but higher bias

- Constant $h_\lambda(x)$ tends to keep the bias of the estimate constant, but the variance is inversely proportional with the local density. Nearest-neighbor window widths exhibit the opposite behavior, the variance stays constant, but the bias is inversely with the density.

- Boundary issues arise. The metric neighborhoods tend to contain less points on the boundaries, while nearest-neighborhood gets wider.

- Compact and non-compact kernel. Epanechnikov kernel, Tri-cube kernel and Gaussian kernel.

### Local linear regression

- Locally weighted average can be biased badly on the boundaries of the domain, because of the asymmetry of the kernel in the region.

- By fitting a locally weighted linear regression we could remove the bias exactly to first order. Actually this bias could also be found in the interior of the domain if the $X$ values are not equally placed. Again, locally weighted linear regression will make a first-order correction.

- Locally weighted linear regression solves a separate for each query point $x_0$:

$$\min_{\alpha_{x_0}, \beta_{x_0}} \sum_{i=1}^N K_\lambda(x_0, x_i) (y_i - \alpha_{x_0} - \beta_{x_0}x_0)^2$$

and estimate $\hat f(x_0) = \hat\alpha_{x_0} + \hat\beta_{x_0}x_0$

- Define $b(x)^T = (1, x)$, $B\in R^{n\times 2}$ with $i$th row $b(x_i)^T$, $W(x_0)$ is $N\times N$ diagnoal matrix with the $i$th diagonal element $K_\lambda(x_0, x_i)$. Then solve

$$\min_{beta} (Y-B\beta)^TW(x_0)(Y-B\beta)$$

gives

$$\hat f(x_0) = b(x_0)^T(B^TW(x_0)B)^{-1}B^TW(x_0)y = \sum_{i=1}^N l_i(x_0)y_i$$

- To prove this estimation 

$$E\hat f(x_0) = \sum_{i=1}^N l_i(x_0)f(x_i) = \sum_{i=1}^N f(x_0)l_i(x_0) + \sum_{i=1}^N f'(x_0)(x-x_0)l_i(x_0) + R$$

And because $\sum l_i(x_0) = 1$, $\sum l_i(x_0)(x_i-x_0) = 0$, hence the bias only depends on quadratic or higher order terms.

- Local linear fits can help bias dramatically at the boundaries at a modest cost in variance. Local quadratic fits do little at the boundaries for bias, but increase the variance a lot.

- Local quadratic fits tend to be most helpful in reducing bias due to curvature in the interior of the domain.

- Asymptotically local polynomials of odd degree dominate those of even degree. This is largely due to the fact that asympototically the MSE is dominated by boundary effects.







