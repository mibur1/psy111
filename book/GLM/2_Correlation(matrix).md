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

# 5.2 Correlation

  For correlation analysis, you have several packages to choose from, such as `NumPy`, `SciPy`, and `pandas`. In this course, we'll focus on the `pandas` package, as it is particularly convenient when working with `pandas` data frames.
```{admonition} Pearson´s correlation
:class: note
Pearson´s correlation coefficient is computed as the ratio of the covariance and the product of the standard deviations `x` and `y`.

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
 Now you can calculate the correlation between them by calling `.corr()` on one Series and passing the other Series as the argument:

 ```{code-cell}
 print(x.corr(y)) 
 ```

By default, the `.corr()` method uses Pearson correlation. However, you can also specify different methods for calculating correlation:
* Pearson (default)
* spearman
* Kendall

```{code-cell}
print(x.corr(y, method='spearman'))  
```

# Correlation Matrices
when trying to compute more than one correlation between variables, it gets very messy without a better way of visualization. For that instance one uses Correlation matrices
First, we make another pandas data frame, but this time with three variables, and we round the results to two decimal places of the correlation
```{code-cell}
#implement data
data = {
    'A': [0, 1, 2, 3, 4, 5, 6, 7, 8, 9],
    'B': [2, 1, 4, 5, 8, 12, 18, 25, 96, 48],
    'C': [9, 7, 8, 6, 5, 4, 3, 2, 1, 0]
}
# Create a pandas Dataframe
df = pd.DataFrame(data)
# Compute the correlation matrix
corr_matrix = df.corr()
# Round the correlation matrix to 2 decimal places
corr_matrix_rounded = corr_matrix.round(decimals=2)
print(corr_matrix_rounded)
```

Now you have your very own correlation matrix. You can enhance the readability of your correlation matrix by converting it into a heatmap using the `seaborn` library's `sns.heatmap()` function. Here’s how you can do it:
Parameters explained:
* `annot=True`: Displays the correlation values inside the heatmap cells.
* `cmap='coolwarm'`: The color map for the heatmap, which you can adjust based on your preference.
* `center=0`: Centers the color map at 0.
* `vmin and vmax`: Define the limits of the color scale, which range from -1 to 1 for correlation coefficients.
* `fmt=".2f"`: Formats the annotation text to 2 decimal places.

```{code-cell}
import seaborn as sns
import matplotlib.pyplot as plt

plt.figure(figsize=(8, 6))
sns.heatmap(corr_matrix_rounded, annot=True, cmap='coolwarm', center=0, vmin=-1, vmax=1, fmt=".2f")
# Set title and labels
plt.title('Correlation Matrix Heatmap')
plt.show()
```
