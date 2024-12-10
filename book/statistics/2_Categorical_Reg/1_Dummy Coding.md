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

# 6.1 Dummy Coding

Dummy coding enables the comparison of each category of a categorical variable to a reference category, which serves as the baseline for measuring the effects of the other categories. In this coding scheme, the reference category is assigned a value of 0 (corresponding to the intercept), while the dummy variables for the remaining categories are assigned values of either 1 or 0. 

To implement dummy coding, we will use a combination of the `patsy` and `statsmodels` package, as this provides straightforward and customizable options, particularly for defining the reference category.

First, we will import the essential libraries needed to visualize the data.

```{code-cell}
import pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt
import statsmodels.formula.api as smf
```

Then we load the data (in this case from a local file) and have a look at it:

```{code-cell}
df = pd.read_csv("data/alzheimers_data.txt", delimiter='\t').dropna()
print(df.head())
```

We then make sure that the `genotype` is treated as a categorical variable. For this, we first check its type:

```{code-cell}
print(df["genotype"].dtypes)
```

We see that it is of type `object`, which is typical for strings. However, for categorical regression, we need to change it to type `category` using `.astype('category')`:

```{code-cell}
df['genotype'] = df['genotype'].astype('category')
print(df["genotype"].dtypes)
```

Great, that worked well. We can now proceed to plot the genotypes as individual boxplots against `WMf`. For this we will use `seaborn` boxplots, as they are easy to use with data frames:

```{code-cell}
sns.boxplot(x='genotype', y='WMf', data=df)
plt.title("WMf for different genotypes")
plt.show()
```

We can then perform categorical regression with dummy coding. For this, we use `statsmodels` combined with the `Treatment` function from the `patsy` package:

```{code-cell}
from patsy.contrasts import Treatment

model = smf.ols('WMf ~ C(genotype, Treatment(reference="e4/e4"))', data=df)
results = model.fit()
print(results.summary())
```

**Outputs:**

When you run the regression `WMf ~ C(genotype, Treatment(reference="e4/e4"))`, the output provides the following key pieces of information:

1. Intercept:
    - This is the mean value of `WMf` for the reference group (`e4/e4` in this case).
    - For example, if the intercept is 0.8185, it indicates that the average WMf for the e4/e4 group is 0.8185.

2. Slopes for Dummy Variables (`C(genotype)[T.<level>]`):
    - Each slope represents the difference in the mean value of `WMf` between the respective `genotype` group and the reference group (`e4/e4`).
    - For example, if `C(genotype)[T.e2/e2]` has a coefficient of -0.0333:
      - The mean `WMf` for the `e2/e2` group is 0.0333 less than the mean for the `e4/e4` group.

3. Regression Equation:
    - The regression equation summarizes the relationship between the predictors (`genotype` groups) and the outcome (`WMf`) by filling in the intercept and the slope specific to each genotype, derived from the output:

$$\hat{Y} = 0.82 - 0.03 * e2/e2 - 0.02 * e2/e3 + 0.06 * e2/e4 + 0.02 * e3/e3 - 0.03 * e3/e4$$

4. Statistical Significance:
    - The p-values for each coefficient test whether the difference between the respective group and the reference group is statistically significant.
    - For example:
      - If `p = 0.668` for `C(genotype)[T.e2/e2]`, it indicates that the mean `WMf` for the `e2/e2` group is not significantly different from the `e4/e4` group, as this p-value is above any reasonable significance threshold (e.g., 0.05).

5. R-squared and Model Fit:
    - The R-squared value indicates how much of the variation in `WMf` is explained by the genotype categories. A higher R-squared value suggests a strong relationship between `genotype` and `WMf`.

## The contrast/design matrix

For dummy coding, there is usually no need to manually create dummy variables or to create a design matrix, as `statsmodels` handles this automatically. However, to ensure that we did everything correctly, we can manually create the contrast matrix and have a look at it:

```{code-cell}
# Get all genotype levels and save them as a list
levels = df['genotype'].cat.categories.tolist()

# Create a Treatment contrast matrix
contrast = Treatment(reference="e4/e4").code_without_intercept(levels)

print("Levels:", levels)
print("Contrast Matrix:\n", contrast.matrix)
```

```{admonition} Summary
:class: tip
- In dummy coding, the reference category is assigned a value of 0
- You can use `model = smf.ols('WMf ~ C(outcome, Treatment(reference="your reference"))', data=my_data)` for dummy coding
```