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

For partial correlation analysis, we will use the `pingouin` package. This package simplifies statistical testing and provides an intuitive interface for various statistical analyses, including partial correlation. However, other packages such as `statsmodels` also implement partial correlations.

```{admonition} Partial Correlation
:class: note
Partial correlation measures the relationship between two variables while controlling for the effect of a third variable. It calculates the correlation between two variables of interest after accounting for the influence of the third variable.
```

## Example: Partial Correlation with the Iris Dataset

We will use the famous **Iris** dataset from the `seaborn` package, which provides data on the physical characteristics of the iris flower. Our goal is to investigate the relationship between petal length and petal width, while controlling for sepal length.

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

The output will include several important metrics:

- **r (Correlation Coefficient)**: This value measures the strength and direction of the linear relationship between petal length and petal width, while controlling for sepal length. In this example, a result of `0.886` suggests a strong positive linear relationship between petal length and width after adjusting for sepal length.
- **CI95% (95% Confidence Interval)**: `[0.85, 0.92]` indicates the range within which the true partial correlation coefficient is likely to fall with 95% confidence. A narrower confidence interval suggests a more precise estimate of the correlation.
- **p-value**: The p-value (`5.26e-51`) indicates the statistical significance of the correlation. A value close to 0 suggests that the observed relationship is highly unlikely to have occurred by random chance. This very small p-value indicates a strong statistical significance.

In conclusion, the partial correlation analysis confirms a strong positive linear relationship between petal length and petal width, independent of the effect of sepal length