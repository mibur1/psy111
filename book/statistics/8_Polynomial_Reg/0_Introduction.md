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

# Polynomial Regression

Polynomial regression extends a linear model by adding higher-degree terms of a predictor variable. This approach captures curved or more complex relationships between a predictor and an outcome, while retaining the core idea of fitting coefficients in a regression framework. The general form of a polynomial model of degree $n$ is:

$$\hat{y} = b_0 + b_1 \cdot x + b_2 \cdot x^2 + ... + b_j \cdot x^{n-1}$$

Even though it incorporates non-linear terms of $x$, the model remains linear in its parameters ($b_0, b_1, \ldots, b_n$), preserving much of the interpretability of ordinary linear regression.