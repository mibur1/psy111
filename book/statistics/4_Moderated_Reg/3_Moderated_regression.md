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

# 4.4 Moderated Regression

Moderated regression is given by the equation:

$$Y= b_0+b_1*X_1+b_2*X_2+b_3*(X_1*X_2)$$

- $Y$ is the predicted value of the dependent variable.
- $b_0$ is the intercept.
- $b_1$ is the coefficient for the independent variable $X_1$.
- $b_2$ is the coefficient for the moderating variable $X_2$.
- $b_3$ is the coefficient for the interaction term (product of $X_1$ and $X_2$).
- $(X_1*X_2)$ is the interaction term.

## Moderated Regression

Moderated regression is used to understand whether and how the relationship between two variables (a predictor $X_1$ and an outcome $Y$) changes at different levels of a third variable (the moderator $X_2$). This is accomplished by including an interaction term $(X_1*X_2)$ in the regression model.

## Moderator Variable

A moderator variable is a type of independent variable that influences the strength and/or direction of the relationship between another independent variable $X_1$ and a dependent variable $Y$. In other words, it "moderates" the relationship between the predictor and the outcome. This means that the effect of $X_1$ on $Y$ is not constant but varies depending on the level or value of $X_2$.

**Research question**: We investigate whether the association between fluid intelligence (`gff`) and figural working memory (`WMf`) is moderated by age (`age`).

Specifically, we regress fluid intelligence ($Y$: `gff`) on centered age, WMf and their product term (`age` $*$ `WMf`).

We will use `ols()` from Statsmodels (as discussed in the lecture on GLM) and define the interaction term beforehand in a variable. We will utilize our centered dataset to obtain a meaningful intercept $b_0$.

```{code-cell}
#import libraries
import pandas as pd
import statsmodels.formula.api as smf
import matplotlib.pyplot as plt
import seaborn as sns

# Load the dataset and subset into smaller dataframe
df = pd.read_csv("data/data.txt", delimiter='\t')
dataframe=df[['age', 'subject', 'WMf', 'gff']]

#centering the variables like before
dat_cen= dataframe.copy()
dat_cen['age_c'] = dataframe['age'] - df['age'].mean()
dat_cen['WMf_c'] = df['WMf'] - df['WMf'].mean()

# %% create an interaction term

dat_cen['interac_age_WMf']= dat_cen['WMf_c']*dat_cen['age_c']

model_interaction = smf.ols(formula='gff ~ WMf_c + age_c + interac_age_WMf', data=dat_cen).fit()

print(model_interaction.summary())
```
**What does the output tell me?**
1. The variables `WMf`, `age`, and their interaction term jointly explain 24.3% of the variance in `gff`.
2. Since the model without the interaction term explained 24% of the variance, we conclude that adding the interaction term results in a relatively small increase in explained variance: $24.3\% - 24\% = 0.3\%$ in `gff`.

The equation for our research question can be written as:

$$\text{gff}=0.44+0.75*\text{WMf}-0.0055*\text{age}+0.018*\text{WMf}*\text{age}$$

The following plot illustrates the relationship between `WMf` and `gff` with `age` as their moderator.

```{code-cell}
import matplotlib.pyplot as plt
import seaborn as sns
import numpy as np

# Create the scatter plot with continuous gradient for age
scatter = sns.scatterplot(data=dataframe, x='WMf', y='gff', hue='age', palette='viridis', legend=None)

# Add a regression line for the overall trend
sns.regplot(data=dataframe, x='WMf', y='gff', scatter=False, color='black')

# Add labels and title
plt.xlabel('Figural Working Memory (WMf)')
plt.ylabel('Fluid Intelligence (gff)')
plt.title('Scatter Plot of Working Memory vs. Fluid Intelligence with Age as Moderator')

# Create a colorbar
norm = plt.Normalize(dataframe['age'].min(), dataframe['age'].max())
sm = plt.cm.ScalarMappable(cmap='viridis', norm=norm)
cbar = plt.colorbar(sm)
cbar.set_label('Age')

# Show the plot
plt.show()

```