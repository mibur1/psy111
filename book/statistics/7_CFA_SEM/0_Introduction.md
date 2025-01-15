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

# CFA & SEM

In the previous session, we introduced Exploratory Factor Analysis (EFA) to extract underlying factors in observed variables. This session will focus on **Confirmatory Factor Analysis (CFA)** and **Structural Equation Modeling (SEM)**. 

Both CFA and SEM are used to test hypotheses about the relationships between observed variables and a smaller number of underlying latent variables. Unlike EFA, which is used when there are no prior assumptions about the factor structure — meaning there is no predefined hypothesis about (1) the number of factors needed or (2) which variables load onto which factors — CFA and SEM require the researcher to specify both the number of factors and the relationships between observed variables and these factors. Additionally, CFA and SEM allow for modeling associations between the latent factors themselves, providing a more structured approach to testing theoretical models.


## Steps of CFA/SEM

1. **Decide on the number of factors:** This usually involves domain knowledge and/or a hypothesis about the constructs.
2. **Set up the loading structure:** Decide which items load onto which factor(s). Again, a underlying hypothesis is required.
3. **Identify the factors:** Since a latent variable is not directly measured, thereby lacking a scale unit, the decision about how to scale/identify latent variables is key in CFA and SEM. Usually, these decisions are not of statistical, but theoretical nature (see lecture for further details). Generally, there are two ways to scale latent variables, (1) fixed loadings and (2) standardized factors.
4. **Evaluate and compare model fit:** To evaluate the specified model, one can interpret model fit measures and/or compare the model fit to the one of other models.


## Difference between CFA/SEM

The difference between CFA and SEM might be confusing. One can say that CFA is a special case of SEM which is defined by not having unidirectional paths present at one level (see lecture and tutorial for further details). In other words, in CFA, there are no unidirectional connections between latent variables **on one level**. There can however be *higher order factors*, i.e. latent variables loading on higher order latent variables. 


## Example

Let's assume we measured *working memory (WM)* with 5 items and *general processing speed (GPS)* with 3 items. We propose that the 5 *WM* items load on a *WM* factor and the 3 *GPS* items load on a *GPS* factor. Thus, we end up with 2 latent variables. One might argue that these latent variables belong to a construct called *intelligence (I)*. Therefore, we let both latent factors load on a **higher order latent factor** which we call *I*. This would all still be consindered as **CFA**. 
However, let's think of it in another way. It could also be that *WM* **predicts** *GPS*. If we specify the model in a way such that *WM* is used as a regressor for *GPS*, this would be considered as **SEM**.


## The Dataset

In this session, we will use the `HolzingerSwineford1939` dataset. The dataset contains mental ability test scores of seventh and eighth grade children from two different schools. Apart from demographic information, nine of the original 26 tests are included in the data set.

- x1, x2 and x3 are indicators for visual ability
- x4, x5 and x6 are indicators for text processing related skills
- x7, x8 and x9 are indicators for speed ability