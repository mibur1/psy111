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

# X.2 EFA in Python 

To compute an EFA in Python we will use the `factor_analyzer` package. This package offers not only EFA functionality but also enables the use of confirmatory factor analysis (CFA). 

### Example dataset



### Setup

The following code chunk installs and loads the needed package.

```{code-cell}
pip install factor_analyzer
from factor_analyzer import FactorAnalyzer
```

### Determine number of factors

## Other packages

Please note that there are also other packages which can be used to apply EFA in Python. However, the `factor_analyzer` package stands out as the most comprehensive and reliable Python package for conducting EFA (Persson & Khojasteh, 2021). Also, its EFA results align with those from the `psych` package in R. 

## References

1. Biggs, J., & Madnani, N. (2019). Factor_analyzer documentation. Release 0.3.1. Jeremy Biggs. Retrieved September 09, 2024, from http://factor-analyzer.readthedocs.io/en/latest/index.html
2. Denis, D. J. (2021). Applied univariate, bivariate, and multivariate statistics using Python: A beginner's guide to advanced data analysis. Wiley. https://onlinelibrary.wiley.com/doi/book/10.1002/9781119578208 https://doi.org/10.1002/9781119578208
3. Persson, I., & Khojasteh, J. (2021). Python Packages for Exploratory Factor Analysis. Structural Equation Modeling: A Multidisciplinary Journal, 28(6), 983–988. https://doi.org/10.1080/10705511.2021.1910037
4. https://www.geo.fu-berlin.de/en/v/soga-py/Advanced-statistics/Multivariate-Approaches/Factor-Analysis/A-Simple-Example-of-Factor-Analysis-in-Python/index.html
5. https://www.datacamp.com/tutorial/introduction-factor-analysis