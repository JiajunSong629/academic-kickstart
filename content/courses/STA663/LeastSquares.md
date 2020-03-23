---
title: Least Square Optimization
linktitle: Least Square
toc: true
type: docs
date: "2019-05-05T00:00:00+01:00"
draft: false
menu:
  example:
    parent: Optimization
    weight: 3

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 3
---

## Calculus

$$L = (y-X\beta)^T(y-X\beta) = y^Ty-y^TX\beta-\beta^TX^Ty+\beta^TX^TX\beta$$

Several things with matrix derivation:
- $$\frac{d \beta^Tb}{d\beta} =  \frac{d  b^T\beta}{d\beta} = b^T$$
- $$\frac{d\beta^Tb}{d\beta^T} =\frac{d b^T\beta}{d\beta^T} = b$$
- $$\frac{d\beta^TA\beta}{d\beta^T} = 2A\beta$$

Traditionally gradient is row vector, so derivation with respect to $\beta^T$ gives us the column gradient

$$\frac{dL}{d\beta^T} = - X^Ty - X^Ty + 2X^TX\beta$$

Gradient Descent

$$\beta_{k+1} = \beta_k - \alpha_k X^T(y-X\beta)$$