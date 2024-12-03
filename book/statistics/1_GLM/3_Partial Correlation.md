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

# 6.3  Partial Correlation

Partial correlation is a statistical measure used to determine the relationship between two variables while controlling for the influence of one or more additional variables. Unlike a simple correlation, which assesses the direct association between two variables, partial correlation isolates the effect of confounding variables to better understand the true relationship.

## Example: Partial Correlation with the Iris Dataset

We will use the `pingouin` package for our partial correlation analysis and apply it to the famous *Iris* dataset, which provides data on the physical characteristics of the iris flower. Our goal is to investigate the relationship between petal length and petal width, while controlling for sepal length

```{code-cell}
# Import the necessary libraries
import seaborn as sns
import pingouin as pg

# Load the iris dataset
df = sns.load_dataset('iris')

# Compute the partial correlation between petal length and petal width,
# controlling for sepal length
partial_corr = pg.partial_corr(data=df,
                               x='petal_length',
                               y='petal_width',
                               covar='sepal_length')
print(partial_corr)
```

## Interpreting the Results:

The output includes several important metrics.

- **r (correlation coefficient)**: This value measures the strength and direction of the linear relationship between petal length and petal width, while controlling for sepal length. In this example, a result of `0.886` suggests a strong positive linear relationship between petal length and width *after* adjusting for sepal length.
- **CI95% (95% confidence interval)**: `[0.85, 0.92]` indicates the range within which the true partial correlation coefficient is likely to fall with 95% confidence. A narrower confidence interval suggests a more precise estimate of the correlation.
- **p-value**: The p-value (`5.26e-51`) indicates the statistical significance of the correlation. A value close to 0 suggests that the observed correlation is statistically highly significant and unlikely to be due to random chance under the null hypothesis.

In conclusion, the partial correlation analysis confirms a strong positive linear relationship between petal length and petal width, independent of the effect of sepal length.

```{admonition} Summary
:class: tip
- Partial correlation measures the relationship between two variables while controlling for the effect of a third variable.
- The `pingouin` package offers a nice and easy way for calculating partial correlations.
```