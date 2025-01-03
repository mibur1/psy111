---
jupytext:
  formats: md:myst
  text_representation:
    extension: .md
    format_name: myst
    format_version: 0.13
    jupytext_version: 1.11.5
kernelspec:
  display_name: Python 3
  language: python
  name: python3
---

# Spline Regression

In the case of highly non-linear associations one might use local (non-parametric) regression models. These include stepwise functions, regression splines, and local regression models. The general idea of these non-linear models is to break the observed range of $X$ into a series of shorter bins and fit different equations across these spans/bins. 

## Stepwise functions

Stepwise functions cut the range of $X$ into bins, and fit a different **constant** within each bin. This is equivalent to converting a continuous variable $X$ into an ordered categorical variable.

By creating $K$ cut points $C_1, C_2, ..., C_k$ in the range of $X$, we construct $k+1$ new variables $(C)$. Don't be confused here, altough the function index runs until $k$, $k+1$ functions are constructed since the index starts at 0 (instead of 1).

$$C_0(X) = I(X < c_1) \\ C_1(X) = I(c_1 \leq X \leq c_2) \\ C_2(X) = I(c_2 \leq X \leq c_3) \\  C_3(X) = I(c_3 \leq X \leq c_4) \\ ...\\  C_{k-1}(X) = I(c_{k-1} \leq X \leq c_K) \\  C_k(X) = I(c_k \leq X)$$ 

Here, $I()$ is a *indicator function*. For example, $I(X < c_1)$ gives 1 if $X < c_1$ and 0 if $X \geq \ c_1$.

The regression equation then looks like this:

$$\hat{y} = b_0 + b_1 \cdot C_1(X) + b_2 \cdot C_2(X) + ... + b_k \cdot C_k(X)$$

Imagine the following case: $c_1 \leq X \leq c_2$. In this case, C_1 is 1 and all other $C()$ give 0. Then we end up with:

$$\hat{y} = b_0 + b_1 \cdot 1$$

## Regression Splines

Regression splines are more flexible than polynomials and stepwise functions, and in fact they are an extension of the two. The idea behind regression splines is to break up the observed span of X into two or more pieces and fit a different constrained polynomial function to each piece (piecewise polynomials).

Instead of fitting a high-degree polynomial to the entire range of $X$, we fit separate low-degree polynomials of different regions of $X$. Imagine this example with a third-degree polynomial and 1 cut point (i.e. 2 regions).

$$
\hat{y}=
\begin{cases}
b_{01} + b_{11} \cdot x + b_{21} \cdot x^2 + b_{31} \cdot x^3, \text{if } x \leq c;\\
b_{02} + b_{12} \cdot x + b_{22} \cdot x^2 + b_{32} \cdot x^3, \text{if } x > c
\end{cases}
$$

If $x \leq c$, the upper polynomial is fitted, otherwise the lower one is fitted. Note the some parameters of the second polynomial ($b_{02}, b_{12}, b_{22}$) have to be constrained to make the function continous and smooth. Please refer to chapter X for details.

```{admonition} Learning break
:class: note

Stepwise regression can be refered to as regression splines (piecewise polynoimals) of the degree...?
```

