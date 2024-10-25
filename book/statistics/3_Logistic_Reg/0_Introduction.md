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

# 3.1 Logistic Regression

Today, we will focus on logistic regression. As you may know from previous lectures, linear regression is not suitable for predicting binary categorical dependent variables (such as _symptom present/absent_ or _task correct/incorrect_) from a quantitative independent variable. To handle this, we use logistic regression.

Logistic regression can be expressed in different forms, including:

1. Conditional probability function
2. Conditional odds (not shown here)
3. Log-odds (logit function)

The seminar will follow this outline:

- The example dataset
- Logistic regression and model evaluation
- The logit transformation
- Exercises