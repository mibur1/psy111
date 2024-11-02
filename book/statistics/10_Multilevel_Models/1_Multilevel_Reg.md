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

# 10.1 Multilevel Regression in Python

To compute an Polynomial Regression in Python we will use the `statsmodels` package again.

## Example dataset

To demonstrate multilevel regression we use a the `sleepstudy` dataset.
The `sleepstudy` dataset is a well-known dataset in the field of mixed-effects modeling, 
often used to illustrate the effects of sleep deprivation on cognitive performance. 
It contains measurements from a study on reaction times of 18 participants 
under sleep deprivation conditions over a period of 10 days. It contain the following variables:

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
##from statsmodels.tools.sm_exceptions import ConvergenceWarning

# Load dataset
data = sm.datasets.get_rdataset("sleepstudy", "lme4").data

# Inspect dataset
print(data)
```

## Plot the data

First, we plot a regression line through all data points, ignoring the clustering variable `Subject`.

```{code-cell}
plt.figure(figsize=(12, 5))
sns.regplot(x='Days', y='Reaction', data=data, ci=None, color='blue')
plt.title("Regression Line for All Data Points")
plt.xlabel("Day")
plt.ylabel("RT")
plt.show()
```

Next, lets plot the regression lines for 10 randomly selected pigs.

```{code-cell}
selected_subj = data['Subject'].drop_duplicates().sample(10, random_state=42)
data_subset = data[data['Subject'].isin(selected_subj)]

plt.figure(figsize=(12, 5))
sns.lmplot(x='Days', y='Reaction', hue='Subject', data=data_subset, ci=None, aspect=1.5)
plt.title("Separate Regression Lines for 10 Randomly Selected Subjects")
plt.xlabel("Day")
plt.ylabel("RT")
plt.show()
```

We see that intercepts an slopes are relatively variable. Some subjects have higher intercept, some have lower ones. Also, while some subjects show a strong relationship between `Days` and `Reaction` (steep slope), some show nearly no relationship. Therfore, ignoring the clustering variable `Subject` may lead to misinterpretations of the data.

## Fit multilevel models

We create a sequence of multilevel models to provide an answer to the four research questions elaborated above.

- Q1: How much variation in `Reaction` is between the `Subject`?

  **Model 1** - The unconditional model = includes no predictors at level-1

- Q2: What is the average relationship between `Days` and `Reaction`?

  **Model** 2 – The random intercept model = includes predictor(s) only at level-1; only intercepts but not slopes are random

- Q3: Does the relationship between `Days` and `Reaction` vary across `Subject`?

- Q4: Is there relationship between the `Reaction` at `Days = 0` and the relationship between `Days` and `Reaction` (i.e., is there a correlation between intercept and slope)?

  **Model 3** – The random intercept and random slope model = includes predictor(s) only at level-1; intercepts and slopes are random

### Model 1 - The unconditional model

Q1: How much variation in `Reaction` is between the `Subject`?

There is no predictor in the unconditional model. Here, we only estimate the intercept of `Reaction` and let it vary across the Level-2 units (`Subject`). The proportion of variance attributed to Level-1 units and Level-2 units can be estimated based on the estimated parameters in this model. In other words, this model can be used to compute the Intra-Class Correlation (ICC) coefficient, which indicates the similarity of observations clustered under the Level-2 units.

```{code-cell}
model1 = smf.mixedlm("Reaction ~ 1", data, groups=data["Subject"])
model1_fit = model1.fit(method="lbfgs")
print(model1_fit.summary())
```

The results indicate:

- Fixed Effects: The average intercept of `Reaction` is X.XXXX, indicated by the `Intercept` value displayed in the output summary. p = .XXXX indicates that this mean is significantly different from 0.

- Random Effects: 

#### Optional

Unfourtunately, there is no function to get the ICC. However, we can quickly write our own ICC function. 

```{code-cell}
def get_icc(results):
    '''get the Intraclass Correlation Coefficient (ICC)'''
    icc = results.cov_re / (results.cov_re + results.scale)
    
    return icc.values[0, 0]

icc = get_icc(results)
print(icc)
```

### Model 2 - The random intercept model

Q2: What is the average relationship between `Days` and `Reaction`?

In the second model, we add the variable `Days` as predictor for `Reaction`. By estimating the average (fixed) slope, this model will inform about the average relationship between `Days` and `Reaction`. In Model 2, there is a predictor `Days` at Level-1 (individual), random intercepts and constant slopes across Level-2 units (`Subject`).

```{code-cell}
model2 = smf.mixedlm("Reaction ~ Days", data, groups=data["Subject"])
model2_fit = model2.fit(method="lbfgs")
print(model2_fit.summary())
```

From the above result, we conclude the average relationship between `Days` and `Reaction` to be X.XXXX, indicating the fixed slope across individuals. More specifically, this coefficient can be interpreted as the expected increase in  `Reaction` across all individuals (`Subject`) provided a 1-unit increase in `Days`. The variance of the random intercept is X.XXXX and the Level-1 residual variance is X.XXXX. 

### Model 3 - The random intercept and random slope model

Q3: Does the relationship between `Days` and `Reaction` vary across `Subject`?

Q4: Is there relationship between the `Reaction` at `Days = 0` and the relationship between `Days` and `Reaction` (i.e., is there a correlation between intercept and slope)?

Model 3 includes the `Days` predictor at Level-1, a random intercept and a random slope. This model can be used to estimate the variance of the slope and answer the question whether the relationship between `Days` and `Reaction` varies across individuals (`Subject`). 

```{code-cell}
model3 = smf.mixedlm("Reaction ~ Days", data, groups=data["Subject"], re_formula="~Days")
model3_fit = model3.fit(method="lbfgs")
print(model3_fit.summary())
```

The variance of the random slope across indiviuals is X.XXXX, indicated by `Days Var` in the summary of results.  