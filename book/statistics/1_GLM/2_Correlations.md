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

# 5.2 Correlations

In correlation analysis, several Python packages can be used, such as `NumPy`, `SciPy`, and `pandas`. For this tutorial, we will focus on the `pandas` package since it is particularly convenient when working with `DataFrames`.

```{admonition} Pearson´s correlation
:class: note
Pearson’s correlation coefficient is calculated as the ratio of the covariance of two variables `X` and `Y` to the product of their standard deviations.

$$r = \frac{\text{Cov}(X, Y)}{s_X s_Y}$$
```

After importing the package, we'll create our own `pandas` data frame to work with.

```{code-cell}
import pandas as pd

x = pd.Series(range(10, 20))
print(x)
```

```{code-cell}
y = pd.Series([2, 1, 4, 5, 8, 12, 18, 25, 96, 48])
print(y)
```

To calculate the correlation between these two series, you can use the `.corr()` method. By default, this method calculates Pearson’s correlation coefficient:

 ```{code-cell}
 print(x.corr(y))
 ```

The `.corr()` method also allows you to specify other correlation methods:

- Pearson (default)
- Spearman
- Kendall

Here’s an example using Spearman’s rank correlation:

```{code-cell}
print(x.corr(y, method='spearman'))
```

## Correlation Matrices

When you want to compute correlations for multiple variables, handling individual correlations can become cumbersome. In such cases, you can use correlation matrices to visualize all correlations at once.

Let’s create a new DataFrame with three variables and compute the correlation matrix, rounding the results to two decimal places:

```{code-cell}
# Define the data
data = {
    'A': [0, 1, 2, 3, 4, 5, 6, 7, 8, 9],
    'B': [2, 1, 4, 5, 8, 12, 18, 25, 96, 48],
    'C': [9, 7, 8, 6, 5, 4, 3, 2, 1, 0]
}

# Create a pandas DataFrame
df = pd.DataFrame(data)

# Compute the correlation matrix
corr_matrix = df.corr()

# Round the correlation matrix to 2 decimal places
corr_matrix_rounded = corr_matrix.round(2)
print(corr_matrix_rounded)
```

Now, you have your own correlation matrix. To make the results more visually appealing and easier to interpret, you can convert the correlation matrix into a heatmap using the seaborn library. Here’s how:

```{code-cell}
import seaborn as sns
import matplotlib.pyplot as plt

# Create the heatmap
sns.heatmap(corr_matrix_rounded, annot=True, cmap='coolwarm', center=0, vmin=-1, vmax=1, fmt=".2f")

# Set the title and show the plot
plt.title('Correlation Matrix Heatmap')
plt.show()
```
Key Parameters for the Heatmap:

- `annot=True`: Display correlation values inside the heatmap cells.
- `cmap='coolwarm'`: Set the color map. You can change this to any other color palette you prefer.
- `center=0`: Center the color map at 0 to emphasize neutral correlations.
- `vmin` and `vmax`: Define the limits of the color scale, which range from -1 to 1 for correlation coefficients.
- `fmt=".2f"`: Format the annotation text to display values with two decimal places.
