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
# 5.3  Partial Correlation
For partial correlation analysis, you can use the `pingouin` package. This package simplifies statistical testing and provides a user-friendly interface for performing various statistical analyses, including partial correlation. While similar analyses can be conducted using `Statsmodels`, `pingouin` offers a more straightforward approach.
```{admonition} Partial Correlation
:class: note
Partial correlation measures the relationship between two variables while controlling for the effect of a third variable. In other words, it calculates the correlation between the two variables of interest after removing the influence of the third variable.
```
As an example, we will use the `iris` dataset from the `seaborn` package which provides us with data on the iris flower. We will investigate the relationship between petal length and petal width while controlling for sepal length.
```{code-cell}
#import the libraries
import seaborn as sns
import pingouin as pg
# Load the iris dataset
df = sns.load_dataset('iris')
# Compute the partial correlation between petal length and petal width, controlling for sepal length
partial_corr = pg.partial_corr(data=df, x='petal_length', y='petal_width', covar='sepal_length')
print(partial_corr)
```
**Results explained:**
* **r (Correlation Coefficient)**: measures the strength of a relationship between two variables while holding a third constant. The results of 0.886 indicate a strong positive linear relationship between petal length and width without the influence of sepal length.
* **CI95% (95% Confidence Interval)**: [0.85, 0.92]: This interval provides a range within which the true partial correlation coefficient is likely to fall with 95% confidence. A narrower interval indicates a more precise estimate.
* **p-value**: This value (5.26e-51) indicates the statistical significance of the correlation (close to 0).