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

# Logistic Regression

In this session, we will focus on **logistic regression**, a method specifically designed to predict binary categorical outcomes (e.g., *symptom present/absent* or *task correct/incorrect*) based on quantitative independent variables. As discussed in previous lectures, linear regression is unsuitable for such tasks because it assumes a continuous dependent variable. Logistic regression addresses this limitation effectively.

## Seminar Outline
1. Logistic regression and model evaluation
2. Understanding the logit transformation
3. Hands-on exercises

## The Data

Logistic regression is specifically designed to handle binary outcomes, making it a more appropriate model for predicting probabilities of categorical events compared to linear regression.

**Research Aim**: Using children's age and theory of mind ability to predict whether they understand display rules or not.

**Context**: In developmental cognitive psychology, theory of mind is defined as the ability to understand the intentions, beliefs, and emotions of others. Display rules refer to social or cultural norms that guide how individuals should express their emotions in different contexts.

To address this research question, children were given a false belief task (a task commonly used to measure their ability to understand others' intentions) and a display rules understanding task, which they could either pass or fail (a dichotomous outcome). Their age in months was also recorded.

In total, 70 children, aged between 24 and 83 months, participated in the study.