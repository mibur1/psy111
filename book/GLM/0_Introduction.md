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

# W4 - Generale (Multivariate) Linear Model (GLM)


In the previous statistics lecture you learnt about the Generale Linear Model. The approach for your first statistical model differs from testing your null hypothesis. Instead of just rejecting or accepting the null hypothesis after defining the aproppiate significance level and statistical test, it´s more of an interactive analysis with GLMs. As you may know from the lecture the formula for simple linear regression is
 
y = mx + b


 BUT we need more specifications to fit the model:</br>
  • the model parameters (here, e.g., $m$ for “multiplier” and $b$ for “bias”) are determined</br>
 • the quality of the model is assessed</br>
 • and the residuals (i.e., the remaining errors) are inspected, to check if the proposed
 model has missed essential features in the data.


So for the Genral linear Model without multiple predictors:

$ y_i = \beta_0 + \beta_1 x_i + \epsilon_i$

...and with multiple ($k$) predictors:

$y_i = \beta_0 + \beta_1 x_{i1} + \beta_2 x_{i2} + \dots + \beta_k x_{ik} + \epsilon_i$

 