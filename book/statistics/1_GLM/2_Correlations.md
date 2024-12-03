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

# 6.2 Correlations

Many Python packages, such as `numpy`, `scipy`, and `pandas` offer functionalities for correlation analysis. As we often work with data in the form of Pandas DataFrames, we will use the `pandas` package for now.

```{admonition} Pearson´s correlation
:class: note
Pearson’s correlation coefficient is calculated as the ratio of the covariance of two variables `X` and `Y` to the product of their standard deviations:

$$r = \frac{\text{Cov}(X, Y)}{s_X s_Y}$$
```

Let's start by creating a simple DataFrame containing an x and y variable:

```{code-cell}
import pandas as pd

data = {
    'X': range(10, 20),
    'Y': [2, 1, 4, 5, 8, 12, 18, 25, 96, 48]
}

df = pd.DataFrame(data)
print(df)

```

We can then calculate the correlation between these two variables by using the `.corr()` method. By default, this method calculates Pearson’s correlation coefficient:

```{code-cell}
correlation_coef = df['X'].corr(df['Y'])
print("Pearson's correlation:", correlation_coef)
```

The `.corr()` method also allows you to use rank correlations (Spearman, Kendall) thrugh the method parameter. For example, you can calculate Spearman’s rank correlation as follows:

```{code-cell}
print("Spearman correlation:",
      df['X'].corr(df['Y'],
      method='spearman'))
```

## Correlation Matrices

If we want to compute correlations for multiple variables, handling individual correlations will not be feasible. In such cases, we can calculate and use correlation matrices, which contain all pairwise correlations. Let’s start by creating a new DataFrame with three variables and then compute the correlation matrix, rounding the results to two decimal places:

```{code-cell}
# Define the data
data = {
    'A': [0, 1, 2, 3, 4, 5, 6, 7, 8, 9],
    'B': [2, 1, 4, 5, 8, 12, 18, 25, 96, 48],
    'C': [9, 7, 8, 6, 5, 4, 3, 2, 1, 0],
    'D': [1, 3, 3, 5, 8, 3, 6, 12, 5, 34]
}

# Create a pandas DataFrame
df = pd.DataFrame(data)

# Compute the correlation matrix
corr_matrix = df.corr()

# Round the correlation matrix to 2 decimal places
corr_matrix_rounded = corr_matrix.round(2)
print(corr_matrix_rounded)
```

To make the results more visually appealing, we can plot the correlation matrix as a heatmap using the `seaborn` library:

```{code-cell}
import seaborn as sns
import matplotlib.pyplot as plt

sns.heatmap(corr_matrix_rounded, 
            annot=True,      # Display correlation values inside the heatmap cells.
            vmin=-1, vmax=1, # Define the limits of the color scale
            square=True,     # Make sure cells stay squar
            cmap="coolwarm", # Set the color map
            linewidths=1     # Lines between the cells
            )
plt.show()
```

```{admonition} Summary
:class: tip
- You can calculate correlations with various packages. In Pandas, you can use the `.corr()` method.
- A nice and intuitive way of visualizing multiple correlation values are correlation matrices (heatmaps).
```