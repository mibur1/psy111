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

# 8.1 Centering Predictors

As we mentioned many times before, it is important to always look at your data! So before we begin with the analysis, lets get a better understanding of the data. For convenience, we create a smaller subset of the main DataFrame:

*Additional information: When creating new DataFrames derived from others, they are by default a view or a shallow copy of the original DataFrame. This is due to Pandas optimizing memory usage and access. However, when a shallow copy is created, the data may not be completely decoupled. Only the structure (index and column information) is copied, but the data itself may still reference the original. Sometimes it can therefore be helpful to make a deep copy, which creates an entirely independent object by using the `.copy()` method.*

```{code-cell}
import pandas as pd

df = pd.read_csv("data/data.txt", delimiter='\t')
df_small = df[['age', 'subject', 'WMf', 'gff']].copy() # Create a deep copy
```

## Descriptive Analysis

We'll start by examining the first few rows of our new subset using `head()` and then apply `describe()` to get an overview of key statistics, such as the mean and standard deviation.

```{code-cell}
print(df_small.head())
print(df_small.describe())
```

To further visualize the data, we can use Seaborn's histplot to examine the frequency distribution of the age variable. This will give us a clearer view of how age is distributed across our sample.

```{code-cell}
#import libraries
import matplotlib.pyplot as plt
import seaborn as sns

sns.histplot(df_small['age'], bins=10);
```

By using Seaborn's 'scatterplot', we can explore the relationship between `WMf` and `gff` and identify any trends. Additionally, we will incorporate age as the hue to visualize how this variable may influence the relationship between `WMf` and `gff`.

```{code-cell}
sns.scatterplot(data=df_small, x='WMf', y='gff', hue='age');
```

## Centering the Predictors

As discussed in the lecture, it is common practice to center predictors in multiple regression around their mean to obtain a meaningful intercept $b_0$. We can do this by subtracting the mean of each variable from their raw values. We will then save the centered variables to new columns:

```{code-cell}
df_small['age_c'] = df_small['age'] - df_small['age'].mean()
df_small['WMf_c'] = df_small['WMf'] - df_small['WMf'].mean()

print(df_small.head())
```

We can now proceed with the actual analysis. To compare multiple linear regression models with centered and non-centered predictors, we can use `ols()` from Statsmodels and plot the results with the different variables. Let's create two identical models, one with non-centered predictors, and one with centered ones:

```{code-cell}
import statsmodels.formula.api as smf

results = smf.ols(formula='gff ~ WMf + age', data=df_small).fit()
print(results.summary())
```

```{code-cell}
results = smf.ols(formula='gff ~ WMf_c + age_c', data=df_small).fit()
print(results.summary())
```

**What's the difference?**

- In the centered model, the intercept can now be interpreted as the expected fluid intelligence at
  1. the average level of figural working memory
  2. the average age of the range considered in the study.

- Centering has **no** influence on the regression slope coefficients (for `WMf` and `age`).
- Centering has **no** influence on the explained variance $(R^2)$.

