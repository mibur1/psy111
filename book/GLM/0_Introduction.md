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


In the previous statistics lecture you learnt about the Generale Linear Model. The approach for your first statistical model differs from testing your null hypothesis. Instead of just rejecting or accepting the null hypothesis after defining the aproppiate significance level and statistical test, it´s more of an interactive analysis with GLMs. As you may know from the lecture:
 y =m∗x+b
 BUT we need more specifications to fit the model:
 the model parameters (here, e.g., m for “multiplier” and b for “bias”) are deter
mined,
 • the quality of the model is assessed,
 • and the residuals (i.e., the remaining errors) are inspected, to check if the proposed
 model has missed essential features in the data.

 (formel einfügen der lecture)
 
 If the visual inspection of the data (plotting) suggests a different model or the residuals are too large, the GLM is rejected and another model might fit better.