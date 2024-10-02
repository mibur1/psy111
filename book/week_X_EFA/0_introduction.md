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

# WX - Exploratory Factor Analysis (EFA)

EFA is used to identify a (generally) small number of factors that explain most of the variance observed in a larger number of manifest variables. Therefore, EFA is a *dimension reduction* method. It helps in understanding the relationships between variables by identifying clusters of variables that are highly correlated with each other but less so with variables in other clusters. 

## Steps of EFA

In Exploratory Factor Analysis (EFA), the objective is to find the optimal number of factors that effectively explain the relationships among a set of observed variables. The main steps involved in this process are:

1. **Determine the number of factors:** This involves adding factors one at a time until the most appropriate solution is found. Essentially, you’re looking for the smallest number of factors that can still explain most of the variance in your data.

2. **Interpret the initial factor loadings:** Once the initial factor solution has been obtained, the factor loadings are examined. At this stage, the factors are assumed to be uncorrelated and there are no restrictions on the loadings.

3. **Rotate factors for better interpretation:** The next step is to transform the initial loadings and factors by rotating them. This is done to obtain a solution that is easier to interpret. Rotations can be either orthogonal, where the factors remain uncorrelated, or oblique, where the factors are allowed to correlate.”

## Terminology

1. **Factor:** A factor is a latent variable describing the associations bewtween the observed variables. At maximum, a EFA model can have as much factors as oberved variables (however, this makes no sense as the goal is dimension *reduction*). Every factor explains a certain variance in observed variables (not the other way around!). 

2. **Factor Loading:** The factor loading is a matrix which shows the relationship of each variable to the underlying factor. In case of orthogonal factors, it shows depicts correlation between the observed variable and the factor. Larger factors loadings correspond to more explained variance.

3. **Eigenvalues:** Each factor has an *Eigenvalue*. It describes how much variance one factor explains over *all observed variables*. 

4. **Communalities:** Each observed variable has a communality. Communalities range from 0 to 1 and describe how much variance of one oberserved variable is explained by *all factors combined*. For orthogonal factors, commonalities are the sum of the squared loadings for each variable. It represents the common variance. 




