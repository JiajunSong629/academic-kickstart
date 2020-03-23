---
title: Gradient Descent
linktitle: Gradient Descent
toc: true
type: docs
date: "2019-05-05T00:00:00+01:00"
draft: false
menu:
  example:
    parent: Optimization
    weight: 2

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 2
---

<hr>

## Exponetially Weighted Average

### Algorithm

$$z_0 = 0$$

$$z_k = \beta z_{k-1} + (1-\beta) y_k$$

Write out and notice that the cofficients are expentially decayed

$$z_k = (1-\beta)y_k + \beta(1-\beta)y_{k-1} + \beta^2(1-\beta)y_{k-2} +... + \beta^{t}(1-\beta)y_{k-t} + ...$$

because

$$(1- (1-\beta))^{\frac{1}{1-\beta}} \approx \frac{1}{e}$$

we would say $z_k$ approximately average over the past $\frac{1}{1-\beta}$ terms which is $y_k, y_{k-1}...$ . [Reference here](
https://www.youtube.com/watch?v=lAq96T8FkTw). The larger $\beta$ will give a smoother fitted curve as it takes average over a longer term.

```Python
z = 0
for i in range(nsteps):
    # update
    z = b*z + (1-b)*y[i]
    # store z
```

Benefits including 1) space efficiency, only need to hold a row vector compared to ordinary gradient descent 2) one line of code, simple implementation 

### Bias correction

$$z_1 = (1-\beta)y_1$$

$$z_2 = (1-\beta)y_2 + \beta(1-\beta)y_1$$

There are obvious bias in initial steps. For large $\beta$, $z_1,z_2$ will be much smaller than the original values $y_1,y_2$.

Instead at each step we store

$$\frac{z_k}{1-\beta^k}$$

we could see for $z_1$

$$\frac{z_1}{1-\beta^1} = y_1$$

$$\frac{z_2}{1-\beta^2} = w_1y_1 + (1-w_1)y_2$$

is much better than raw $z_k$.

As $t$ gets larger, $\beta^t$ will be close to 0, which makes the bias correction close to the raw ones. Therefore we could apply bias correction to get better warm-up steps.

```Python
z = 0
for i in range(nsteps):
    # update
    z = b*z + (1-b)*y[i]
    zc = z / ((1-b)** (i+1))
    # store zc
```

### Gradient Descent with momentum

$$z_k = \beta z_{k-1} + (1-\beta)\nabla f(x_k)$$

$$(z_k = z_k / (1-\beta^k) \textrm{ if bias correction})$$

$$x_{k+1} = x_k - \alpha z_k$$

Exponetially weighted average can damp out oscillations while maintaining the principal direction heading to the minimum.

```Python
z = 0
for i in range(nsteps):
    z = b*z + (1-b)*grad(x)
    zc = z / (1-b**(i+1))
    x = x - a * zc
    # store x as the ith step position
```

### RMSprop

To damp out oscillations, RMSprop (root mean squared prop) encourages large step in promising direction while making steps in other directions slower.

To do so, RMSprop scales the learning rate in each direction by the square root of the exponentially weighted sum of squared gradients. 

$$s_0 = 0$$

$$s_k = \beta s_{k-1} + (1-\beta)\nabla f(x_k) ^2$$

$$x_{k+1} = x_k - \alpha \frac{\nabla f(x_k)}{\sqrt{s_k}}$$

```Python
s = 0
for i in range(nsteps):
    s = b*s + (1-b) * grad(x)**2
    x = x - a * grad(x) / np.sqrt(s)
    # store x as the ith step position
```

### Adam

Adam optimizor, Adaptive moment estimator, gets its name by using first (momentum) and second (rmsprop) moment.

$$z_0 = 0, s_0 = 0$$

$$z_k = \beta_1 z_{k-1} + (1-\beta_1)\nabla f(x_k)$$

$$s_k = \beta_2 s_{k-1} + (1-\beta_2)\nabla f(x_k)^2$$

$$z_k^c = z_k / (1-\beta_1^k)$$

$$s_k^c = s_k / (1-\beta_2^k)$$

$$x_k = x_{k-1} - \alpha \frac{z_k^c}{\sqrt{s_k^c} + \epsilon}$$

Usually
- $\alpha$ needs to be tuned
- $\beta_1 = 0.9$, $\beta_2 = 0.999$
- $\epsilon = 1e-8$