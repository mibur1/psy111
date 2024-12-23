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

# 9.1 The semopy package

The `semopy` (**S**tructural **E**quation **M**odels **O**ptimization in **Py**thon) package is an excellent tool for Structural Equation Modeling (SEM) and offers a wide range of intuitive modeling options. With Semopy, you can easily define and estimate complex relationships between observed and latent variables, making it ideal for SEM tasks.

The basic workflow looks like this:

1. Define the model: First, you have to define the model using a string-based syntax that specifies the relationships between variables. This can include both observed (measured) variables and latent (unobserved) variables.
2. Fit the model: Once the model is defined, you can fit it to the data to estimate the parameters based on the observed data.
3. Evaluate the model: After fitting the model, you can evaluate its fit using various statistical indices such as the Chi-Square test to assess how well the model explains the data.
4. Plot the model e.g. trough `graphviz` (optional)

## The semopy syntax

### Regression

Regression equations can be specified similar to how you already learned in the previous sessions in e.g. statsmodels: `Y ~ x1 + x2 + x3`:

- The `~` operator indicates that $Y$ is being regressed onto $x_1$​, $x_2$​, and $x_3$​. 
- The `+` operator is used to combine multiple predictors in the model.

### Variance and covariance

The `~~` syntax is used for both covariance between two variables and variance of a single variable:

- For covariance, `x1 ~~ x2` indicates that $x_1$​ and $x_2$​ are allowed to covary.
- For variance, `x1 ~~ x1` indicates the variance of `x_1`​.

