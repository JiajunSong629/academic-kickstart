---
title: Optimization Algorithms
linktitle: Optimization Algorithms
toc: true
type: docs
date: "2019-05-05T00:00:00+01:00"
draft: false
menu:
  example:
    parent: Optimization
    weight: 4

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 4
---

<hr>

# Univariate optimization

## Interval elimination
- Golden section search
- interpolation
near a minimum, from taylor series we know that the function will look like a quadratic. for $ax^2+bx+c$ the minimum is given by $-\frac{b}{2a}$
- newton method
$$x_{k+1} = x_k - \frac{f'(x_k)}{f''(x_k)}$$
- quasi-newton method
replace the second derivatives with finite approximation, the secant of the first derivatives

# Algorithms for optimization for mutiple problems

- Convexity guarantees that if an optimizer converges, it converges to the global minimum.

## Line search methods
- we search on a line (in $n$ dimensions) and try to find a minimum.
$$x_{k+1} = x_k + \alpha_kp_k$$
- step size
choose $\alpha$ to minimize
$$\psi(\alpha) = f(x_k+\alpha p_k)$$
wolfe conditions to ensure $f$ decrease sufficiently

### Steepest descent

Choose $p_k = \nabla f(x_k)$
Consider a quadratic
$$f(x) = \frac{1}{2}x^TQx - b^Tx$$
$$\nabla f(x) = Qx-b$$
$$f(x_k - \alpha \nabla f_k) = \frac12\left(x_k - \alpha \nabla f_k\right)^TQ\left(x_k - \alpha \nabla f_k\right) - b^T \left(x_k - \alpha \nabla f_k\right) $$
$$\alpha_k = \frac{\nabla f_k^T\nabla f_k}{\nabla f_k^TQ\nabla f_k}$$
$$x_{k+1} = x_k - \frac{\nabla f_k^T\nabla f_k}{\nabla f_k^TQ\nabla f_k} \nabla f_k$$
$$||x_{k+1} - x_0||_Q^2 \leq \left(\frac{\lambda_n - \lambda_1}{\lambda_n+\lambda_1}\right)^2 ||x_{k} - x_0||_Q^2$$

convergence slows as $\frac{\lambda_n}{\lambda_1}$ increases.

### Newton's method
- Choose $p_k = -H^{-1}\nabla f(x_k)$
- if Hessian is not positive definite, it may not be a descent direction.
- in the neighbourhood of a local minimum, the Hessian will be positive definite, and step size $\alpha_k=1$ gives quadratic convergence
- the advantage of multiply gradient with the Hessian is that the gradient is corrected for curvature, and the new direction points to the minimum

### Coordinate Descent

The main advantage is that $\nabla f$ is not required. It can behave reasonably well, if coordinates are not tightly coupled.

### Newton CG algorithm

Minimizes a 'true' quadratic on $R^n$ in $n$ steps
Does NOT require storage or inversion of an $n \times n$ matrix.

- Approx a function with
$$f(x+a) \approx f(x) - a^Tb + \frac{1}{2}a^THa$$
then by finding the minimum is to find the linear equation
$$Hx = b$$
- current location $x_k$, find the direction vector by conjugate projecting $\nabla f(x_k) = Hx_k-b$ on previous vectors space

$$p_{k+1} = \nabla f(x_{k+1}) - \sum\limits_{i=1}^{k}\frac{p_i^T A \nabla f(x_k)}{p_i^TAp_i} p_i$$

- The 'trick' is that in general, we do not need all $n$ conjugate vectors. In fact, it turns out that $\nabla f(x_k) = b-Ax_k$ is conjugate to all the $p_i$ for $i=1,...,k-1$. Therefore, we need only the last term in the sum.

$$p_{k+1} = \nabla f(x_{k+1}) - \frac{p_k^TA\nabla f(x_k)}{p_k^TAp_k}p_{k}$$

- find the optimal step size $\alpha_k$, by setting the first derivatives w.r.t $\alpha$ to zero, update next location as

$$x_{k+1} = x_k + \alpha_kp_k$$

- return to step 1

### BFGS - Broyden–Fletcher–Goldfarb–Shanno
- do not need to compute Hessian matrix in Quasi-Newton method, iteratively update the approximation of Hessian
- secant

### Nelder-Mead Simplex

- zero-order method, only depends on the function itself
- [a vivid animation](https://www.youtube.com/watch?v=j2gcuRVbwR0)

### Powell's method
- [a short animation](https://www.youtube.com/watch?v=4TYJGihyuDg)

## Solvers

### Levenberg-Marquardt (Damped Least Squares)

Least squares problem:

$$S(\beta) = \sum_{i=1}^m (y_i - f(x_i,\beta)^2)$$

- Newton method

$$\beta_{k+1} = \beta_k - H(\beta_k)^{-1}\nabla S(\beta_k)$$

- Gradient Descent method

$$\beta_{k+1} = \beta_k - \alpha_k \nabla S(\beta_k)$$

- Levenberg-Marquardt

$$\beta_{k+1} = \beta_k - (H+\lambda_kI)^{-1}\nabla S(\beta_k)$$


When $\lambda_k$ is small, the update is essentially Newton-Gauss, while for $\lambda_k$ large, the update is gradient descent.


### Newton-Krylov


## GLM Estimation and IRLS
