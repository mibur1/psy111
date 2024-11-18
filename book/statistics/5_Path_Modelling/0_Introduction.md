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

# Path Modelling

Today, we'll implement path modeling in Python using the `semopy` package. We will start by formulating hypotheses about causal relationships between variables. These hypotheses will help us estimate a system of directed effects, based on an observed correlation matrix of multiple variables. 
This package is quite powerful, and you'll have the opportunity to use it in upcoming lectures for more advanced techniques, such as Structural Equation Modeling (SEM) and Confirmatory Factor Analysis (CFA).

Here is todays outline:
- `semopy` syntax
- Health example
- Exercises