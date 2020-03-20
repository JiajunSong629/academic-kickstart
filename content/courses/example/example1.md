---
title: Root finding
linktitle: Root finding
toc: true
type: docs
date: "2019-05-05T00:00:00+01:00"
draft: false
menu:
  example:
    parent: Example Topic
    weight: 1

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 1
---

<hr>

## Calculus review

- one function $f$, one variable $x$

$$f(x+\Delta x) \approx f(x) + (\Delta x)\frac{df}{dx}(x)+\frac{1}{2}(\Delta x)^2\frac{d^2f}{dx^2}(x)$$

- one function $f$, n variables $x$

$$f(x+\Delta x) \approx f(x) + (\Delta x)^T\nabla f+\frac{1}{2}(\Delta x)^TH(\Delta x)$$
 
- m functions $f_1,\dots f_m$, n variables $x$

$$f(x+\Delta x) \approx f(x) + J(x)\Delta x$$
 
- Gradient is the first derivatives of scalar-valued multi-variables

- Jacobi is the first derivatives of vector-valued multi-variables; m equations, n variables, $R^n$ to $R^m$

- Hessian is the second derivatives of scalar-valued multi-variables

- Hessian matrix $H$ is the Jacobian matrix $J$ of the gradient $\nabla f$
 
 
## Root finding
 
### Bisection method
 
- We can terminate the process whenever the function evaluated at the new midpoint is 'close enough' to zero.
- bracketed methods. This means the root is 'bracketed' by the end-points (it is somewhere in between). Another class of methods are 'open methods' - the root need not be somewhere in between the end-points (but it usually needs to be close!)

### Secant method

```python
def secant_method(f, x0: int, x1: int, iterations: int) -> float:
    """Return the root calculated using the secant method."""
    xs = [x0, x1] + iterations * [None]  # Allocate x's vector

    for i, x in enumerate(xs):
        if i < iterations:
            x1 = xs[i+1]
            xs[i+2] = x1 - f(x1) * (x1 - x) / (f(x1) - f(x))
    
    return xs[-1]  # Return the now filled last entry
```

- covergence rate around $\frac{1+\sqrt 5}{2}$

- methods of false positions: variant of secant method, instead of always choosing the last two values, it picks the most two recent values maintain the bracket property.


### Newton-Raphson method

$$x_{k+1} = x_k - \frac{g(x_k)}{g'(x_k)}$$

- Similar to Secant method, only set the slope to the derivatives and do not need two points.

- Convergence rate is $x^2$ but it may stumble on horizontal asymptote, fail to converge.

$$\epsilon_{k+1} = \epsilon_k - \frac{g(x_k)}{g'(x_{k})}$$

$$g(x_k) \approx g'(x_0)\epsilon_k + 1/2 g''(x_0)\epsilon_k^2 + o(\epsilon_k^2)$$

$$g'(x_k) \approx g'(x_0) + g''(x_0)\epsilon_k$$

Approximate with these and have

$$\epsilon_{k+1} =\frac{g''(x_0)}{2g'(x_0)} \epsilon_k^2$$

### Gaussian-Newton method
$$x_{n+1} = x_n - (J^TJ)^{-1}J^T f(x_n)$$

In multivariate nonlinear estimation problems, we can find the vector of parameters $\beta$ by minimizing the residuals $r(\beta)$, 
$$\beta_{n+1} = \beta_n - (J^TJ)^{-1}J^T r(\beta_n)$$
where the entries of the Jacobian matrix $J$ are
$$J_{ij} = \frac{\partial r_i(\beta)}{\partial \beta_j}$$

### Learning from data notes
- Newton-Raphson method
The gradient $\nabla F$ is often well estimated by using its first derivatives, which is the Hessian matrix $H$

$$\nabla F(x_{k+1}) \approx \nabla F(x_k) + H(x_k)(x_{k+1}-x_k)$$

left hand side to be zero

$$H(x_k)(\Delta x_k) = -\nabla F(x_k), x_{k+1}=x_k+\Delta x$$

- Gauss-Newton method
$$E(\beta_1\dots \beta_k)=\sum_{i=1}^m (y_i-f(x_i,\beta))^2=r^Tr$$

Let $J, H$ be the Jacobian and Hessian matrix of $r$.

$$r(\beta+\Delta\beta) = r(\beta) + J\Delta \beta$$

$$
E(\beta+\Delta\beta) = E(\beta)+(\Delta\beta)^T(J^Tr)+(\Delta\beta)^T(2J^TJ)(\Delta\beta)
$$
we know
$$\nabla E = 2J^Tr$$
$$\Delta E = 2J^TJ$$
To minimize E, i.e. find $\nabla E=0$
$$\nabla E(\beta_{k+1}) \approx \nabla E(\beta_k) + H(x_k)(\beta_{k+1}-\beta_k)$$
Let left side be zero, the iteration is
$$H(x_k)(\beta_{k+1}-\beta_k) \approx \nabla E(\beta_k)$$
$$J(x_k)^TJ(x_k)(\beta_{k+1}-\beta_k) = J(x_k)^Tr(x_k)$$

### Inverse quadratic interpolation
- similar to the secant method
- need three points as a start to interpolate on inverse quadratic function
- inverse to make sure the function touches x axis

```python
from scipy.interpolate import interp1d
interp1d(y0, x0,kind='quadratic')
```

### Brentq method
- a combination of bisection, secant and inverse quadratic interpolation
- the method begins by using the secant method to obtain a third point cc, then uses inverse quadratic interpolation to generate the next possible root
```python
from scipy import optimize
scipy.optimize.brentq(f,left, right)
```

### Roots of polynomial

$$p(x) = a_0 + a_1x + a_2 x^2 + \ldots + a_m x^m$$

the companion matrix is

$$
A = \begin{matrix}
-a_{m-1}/a_m & -a_{m-2}/a_m & \ldots & -a_0/a_m 
1 & 0 & \ldots & 0 
0 & 1 & \ldots & 0
\vdots & \vdots & \ldots & \vdots
0 & 0 & \ldots & 0
\end{matrix}
$$

The roots we are seeking are the eigenvalues of the companion matrix.
