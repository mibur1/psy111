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

# 14.1 Multilevel Regression

To demonstrate multilevel regression models, we use a the `sleepstudy` dataset from `statsmodels`. This is a well-known dataset in the field of mixed-effects modeling often used to illustrate the effects of sleep deprivation on cognitive performance. It contains measurements from a study on reaction times of 18 participants under sleep deprivation conditions over a period of 10 days and contains the following variables:

- `Reaction` - the reaction time in milliseconds, which serves as the outcome variable
- `Days` - the number of days the participant has been sleep-deprived, ranging from 0 to 9
- `Subject` - A unique identifier for each participant, allowing for random effects in the model

```{code-cell}
# Load packages
import numpy as np
import pandas as pd
import statsmodels.api as sm
import statsmodels.formula.api as smf
import seaborn as sns
import matplotlib.pyplot as plt

# Load dataset
data = sm.datasets.get_rdataset("sleepstudy", "lme4").data
print(data.head())
```

## Plot the data

First, we plot a regression line through all data points, ignoring the individual subjects:
```{code-cell}
sns.lmplot(x='Days', y='Reaction', data=data)
plt.title("Combined regression");
```

Next, let's plot one for each subject, using `Subject` as the hue:

```{code-cell}
sns.lmplot(x='Days', y='Reaction', hue='Subject', data=data)
plt.title("Subject-specific regressions");
```

We can clearly see that the intercepts and slopes show significant variability. Some subjects have higher intercepts, some have lower ones. Some subjects show a strong relationship between `Days` and `Reaction` (steep slope), some show nearly no relationship. Therfore, ignoring the clustering variable `Subject` may lead to misinterpretations of the data.

## Fitting multilevel regression models

In the following, we will create three multilevel regression models to provide an answer to the following research questions:

- **Q1:** How much variation in `Reaction` is between the `Subject`?  

  - **Model 1:** The unconditional model - includes no predictors at level 1

- **Q2:** What is the average relationship between `Days` and `Reaction`?

  - **Model 2:** The random intercept model - includes predictor(s) only at level 1; only intercepts but not slopes are random

- **Q3 and Q4:** Does the relationship between `Days` and `Reaction` vary across `Subject`? Is there a relationship between the `Reaction` at `Days = 0` and the relationship between `Days` and `Reaction` (i.e., is there a correlation between intercept and slope)?

  -  **Model 3:** The random intercept and random slope model - includes predictor(s) only at level 1; intercepts and slopes are random

### Model 1 - The unconditional model

Q1: How much variation in `Reaction` is between `Subject`?

There is no predictor in the unconditional model. Here, we only estimate the intercept of `Reaction` and let it vary across the Level-2 units (`Subject`). The proportion of variance attributed to Level-1 units and Level-2 units can be estimated based on the estimated parameters in this model. In other words, this model can be used to compute the Intra-Class Correlation (ICC) coefficient, which indicates the similarity of observations clustered under the Level-2 units.

```{code-cell}
# define and fit the unconditional model
model1 = smf.mixedlm("Reaction ~ 1",
                     data=data,
                     groups=data["Subject"])

# fit the model using the BFGS optimization method
model1_fit = model1.fit(method="bfgs")

# print the summary of the model
print(model1_fit.summary())
```

The summary provides two coefficients:

- `Intercept`: The intercept (298.508) represents the estimated average reaction time at Day 0 (baseline level of `Reaction`) across all subjects. As indicated by the p-value, the intercept is significantly different from zero (p = 0.000). This is the *fixed effect*.
- `Group Var`: The variance of the random intercept is 1278.324. This value indicates how much individual subjects vary in their average reaction times at baseline. This is the *random effect*.

#### Intraclass Correlation Coefficient (ICC)
There is no included function to get the ICC. However, we can quickly code it ourselves:

```{admonition} Intraclass Correlation Coefficient (ICC)
:class: tip 

The ICC represents the proportion of the total variation in the outcome that can be explained by differences between groups, rather than differences within groups.

**Formula:**  

$$ 
\text{ICC} = \frac{\sigma^2_{\text{between}}}{\sigma^2_{\text{between}} + \sigma^2_{\text{within}}}
$$ 
```

```{code-cell}
# create function to calculate ICC
def calculate_icc(results):
    icc = results.cov_re / (results.cov_re + results.scale)
    return icc.values[0, 0]

# call the function with your fitted model to calculate ICC
calculate_icc(model1_fit)
```

An ICC of 0.39 indicates that 39% of the variance in `Reaction` is due to inter-individual differences.

### Model 2 - The random intercept model

Q2: What is the average relationship between `Days` and `Reaction`?

We now add the variable `Days` as predictor for `Reaction`. By estimating the average (fixed) slope, this model will inform about the average relationship between `Days` and `Reaction`. In this model, there is a predictor `Days` at Level-1 (individual), random intercepts and constant slopes across Level-2 units (`Subject`).

```{code-cell}
# define and fit the random intercept model
# this model includes "days" as a predictor at level 1
model2 = smf.mixedlm("Reaction ~ Days",
                     data=data,
                     groups=data["Subject"])

# fit the model using the BFGS optimization method
model2_fit = model2.fit(method="bfgs")

# print summary 
print(model2_fit.summary())
```

The summary provides three coefficients:

- `Intercept`: The intercept (251.405) represents the estimated average reaction time at baseline (days = 0), across all subjects. This is significantly different from zero (p = 0.000).
- `Days`: The average relationship (slope) between `Days` and `Reaction`. This indicates that, on average, for each additional day, 
the reaction time increases by approximately 10.47 ms. This is significantly different from zero (p = 0.000).
- `Group Var`: The variance of the random intercept is 1378.232. This value indicates how much individual subjects vary in their average reaction times at baseline. A higher variance suggests greater variability among subjects’ intercepts, meaning individual differences play a significant role in determining reaction times.

### Model 3 - The random intercept and random slope model

Q3: Does the relationship between `Days` and `Reaction` vary across `Subject`?

Q4: Is there relationship between the `Reaction` at baseline and the relationship between `Days` and `Reaction` (i.e., is there a correlation between intercept and slope)?

Model 3 includes the `Days` predictor (level 1), a random intercept, and a random slope. This model estimates the variance of the slope and determines whether the relationship between `Days` and `Reaction` varies across individuals (`Subject`). We now additionally provide `re_formula` as a one-sided random effects formula defining the variance structure of the model.This specifies that random effects are modeled as a function of `Days`, allowing each subject to have its own intercept and slope.

```{code-cell}
# define the random intercept and random slope model
# model includes "days" as a predictor at level 1 
model3 = smf.mixedlm("Reaction ~ Days",    
                     data=data,
                     groups=data["Subject"],
                     re_formula="~Days")        #random effects

# fit the model
model3_fit = model3.fit(method="bfgs")

# print summary
print(model3_fit.summary())
```

We now get additional coefficients (`Group x Days Cov` and `Days Var`):

- `Days Var`: Represents the variance of the random slopes for the `Days` variable across `Subject`. Some subjects have a steeper increase in reaction time over days, while others may have a slower or negligible increase. A high variance thus indicates more variability among individuals in how their reaction times change over days. 
- `Group x Days Cov`: Describes the covariance between the random intercept and the random slope for `Days`. The positive covariance indicates that subjects with higher baseline reaction times (intercepts) also tend to have larger increases in reaction time over days (steeper slopes).