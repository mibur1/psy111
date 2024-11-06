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

# 8.2 Centering predictors
Sure! Here it is with the LaTeX syntax included:

As discussed in the lecture, it is common practice to center predictors in multiple regression around their mean to obtain a meaningful intercept $b_0$. We can manually do this by subtracting the mean of each variable from their raw values.

We will create a copy of the existing DataFrame and add our new centered variables to it.
```{code-cell}
#import libraries
import pandas as pd

# Load the dataset and subset into a smaller dataframe
df = pd.read_csv("data/data.txt", delimiter='\t')

dataframe=df[['age', 'subject', 'WMf', 'gff']]

# copy of the data frame
dat_cen= dataframe.copy() #by using.copy() you ensure, that you are working with a copy and avoid any warnings

# centering predictors by substracting their mean
dat_cen['age_c'] = dataframe['age'] - df['age'].mean()
dat_cen['WMf_c'] = df['WMf'] - df['WMf'].mean()

#print head of the new data frame
dat_cen.head()
```
After that, we can proceed with the actual analysis.

## Comparing Centered and Non-Centered Regression

To compare multiple linear regression models with centered and non-centered predictors, we can use `ols()` from Statsmodels and plot the results with the different variables. This will allow us to observe the differences in the regression coefficients and the intercepts between the two models.

**Non-Centered Predictors**
```{admonition} Remember:
:class: attention
You can add multiple predictors to `ols()` by using `+`.
```
We will first start with our standard linear regression using the non-centered variables.

```{code-cell}
# %% multiple linear regression with non_centered predictors
import statsmodels.formula.api as smf

model_noncentered = smf.ols(formula='gff ~ WMf + age', data=dat_cen).fit()
print(model_noncentered.summary())
```
**Centered Predictors**

After that, we will use our centered variables for the same regression analysis. This will allow us to compare the results with those obtained from the non-centered predictors.

```{code-cell}
model_centered = smf.ols(formula='gff ~ WMf_c + age_c', data=dat_cen).fit()
print(model_centered.summary())
```
**What does (not) change with Centered Predictors?**
- There is no influence on the regression slope coefficients (for `WMf` and `age`).
- There is no influence on the explained variance  $R^2$.

- The _intercept_ can be interpreted as the expected fluid intelligence at the average level of figural working memory and the average age of the range considered in the study.
