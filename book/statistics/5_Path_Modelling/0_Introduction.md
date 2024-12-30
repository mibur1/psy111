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

Today, we will implement path modeling in Python using the `semopy` package. We will start by formulating hypotheses about causal relationships between variables. These hypotheses will help us estimate a system of directed effects, based on an observed correlation matrix of multiple variables.

The `semopy` package is quite powerful, and we will also use it in the upcoming sessions on Structural Equation Modeling (SEM) and Confirmatory Factor Analysis (CFA).

## The Dataset

In this session, we’ll explore a new dataset to apply path modeling, building upon concepts from previous sessions. This dataset provides an opportunity to examine a well-documented health model, offering insights into the interplay between physical, functional, and subjective health.

Our focus is on understanding the relationships between these health constructs as outlined by Whitelaw and Liang (1991), using path modeling to test the proposed theoretical framework.

## Research Question

How do physical health, functional health, and subjective health relate to each other? Specifically, we aim to investigate the following hypotheses:

- Physical health influences functional health, which, in turn, predicts subjective health.
- Physical health directly affects subjective health.