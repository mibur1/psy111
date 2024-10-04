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

# WX2 - Confirmatory Factor Analysis (CFA) & Structural Equation Modeling (SEM)

CFA is used when to goal is determine a limited number of latent variables from a larger number of measured variables. In comparison to EFA, where we had no underlying assumptions about the factor structure (i.e., we had no hypothesis about (1) how many factors we need and (2) no hypothesis about which variables load onto which factor). In CFA/SEM we have to specify how many factors we want and also which variables load onto which factor(s). Furthermore, we can also specfiy associations between factors. 

## Steps of CFA/SEM

1. **Decide on the number of factors:** This usually involves domain knowledge and/or a hypothesis abou the constructs.
2. **Set up the loading structure:** Decide which items load onto which factor(s). Again, a underlying hypothesis is required.
3. **Identify the factors:** Since a latent variable is not directly measured, thereby lacking a scale unit, the decision about how to scale/identify latent variables is key in CFA and SEM. Usually, these decisions are not of statistical, but theoretical nature (see lecture for further details). Generally, there are two ways to scale latent variables, (1) fixed loadings and (2) standardized factors.
4. **Evaluate and compare model fit:** To evaluate the specified model, one can interpret model fit measures and/or compare the model fit to the one of other models.

## Difference between CFA/SEM

The difference between CFA and SEM might be confusing. One can say that CFA is a special case of SEM which is defined by not having unidirectional paths present at one level (see lecture and tutorial for further details). 