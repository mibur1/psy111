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

# 12.1 Polynomial Regression

Let’s consider a hypothetical situation in which we want to predict an exam score (0–100%) from the number of hours studied per day. A purely linear model might miss an important “sweet spot,” since studying too many hours can lead to fatigue or burnout. By adding a quadratic term, we can model a peak in performance:

- `learn` - Hours learned per day
- `grade` - Exam grade (from -100 to 100)

```{code-cell}
import numpy as np
import pandas as pd

# Simulate the dataset
study_time = np.linspace(0, 10, 500)
h = 6
k = 80
grades = -(k / (h**2)) * (study_time - h)**2 + k + np.random.normal(0, 8, study_time.shape)

study_df = pd.DataFrame({'study_time': study_time, 'grade': grades})
print(study_df.head())
```

Lets plot the relationship between the variables:

```{code-cell}
import seaborn as sns
import matplotlib.pyplot as plt

fig, ax = plt.subplots(figsize=(8,5))
sns.scatterplot(data=study_df, x='study_time', y='grade', alpha=0.6, ax=ax)
ax.set(xlabel="Learn [h]",
       ylabel="Grade [%]",
       title="Scatter Plot: Study-time vs. Grade");
```

It is visible that a non-linear component is present in our simulated data.

## Detecting curvilinear relations

To perform polynomial regression, we will use the `statsmodels` and `sklearn` packages.

### Fit a linear model

To begin with, lets fit a simple linear model. Note that we are already using the function which we will later use to fit higher-order polynomials. Here we set the order to 1. Note that a polynomial with the order 1 is actually a linear model.

```{code-cell}
import statsmodels.api as sm
from sklearn.preprocessing import PolynomialFeatures

polynomial_features_p1 = PolynomialFeatures(degree=1, include_bias=True)
study_time_p1 = polynomial_features_p1.fit_transform(study_time.reshape(-1, 1))

linear_model = sm.OLS(grades, study_time_p1).fit()
linear_fit = linear_model.predict(study_time_p1)
linear_residuals = linear_model.resid
```

Next, lets plot the model and the model residuals:

```{code-cell}
fig, ax = plt.subplots(1, 2, figsize=(8,4))

sns.scatterplot(x=study_time, y=grades, color='blue', alpha=0.5, ax=ax[0])
ax[0].plot(study_time, linear_fit, color='red', linewidth=2)
ax[0].set_title('Linear Regression')

sns.scatterplot(x=study_time, y=linear_residuals, color='red', alpha=0.5, ax=ax[1])
ax[1].axhline(0, linestyle='--')
ax[1].set_title('Residuals');
```

From the upper plot one can already see that whilst there being a linear trend present in the data, the model underestimates the complexitiy of the relationship. Looking at the residuals, it becomes clear that once the strong positive linear trend in the data has been removed, the curvilinearity stands out! The residuals are systematically related to the value of X: below zero for low and high values of X, and above zero for moderate values of X. This is the graphical diagnosis for the existence of a non-linear relationship, higher than degree “1”.

## Fit a polynomial regression

To improve model fit, lets inlcude higher order polynomials. To begin with, lets include a quadratic coefficient, making the polynomial a second order one. First, we use the `sklearn` package to generate the polynomial features.

```{code-cell}
polynomial_features_p2 = PolynomialFeatures(degree=2, include_bias=True)
study_time_p2 = polynomial_features_p2.fit_transform(study_time.reshape(-1, 1))
```

Next, fit the new model and extract its residuals.

```{code-cell}
quadratic_model = sm.OLS(grades, study_time_p2).fit()
quadratic_fit = quadratic_model.predict(study_time_p2)
quadratic_residuals = quadratic_model.resid
```

Lets also plot our new model and its residuals.

```{code-cell}
fig, ax = plt.subplots(1, 2, figsize=(8,4))

sns.scatterplot(x=study_time, y=grades, color='blue', alpha=0.5, ax=ax[0])
ax[0].plot(study_time, quadratic_fit, color='red', linewidth=2)
ax[0].set_title('Quadratic Regression')

sns.scatterplot(x=study_time, y=quadratic_residuals, color='red', alpha=0.5, ax=ax[1])
ax[1].axhline(0, linestyle='--')
ax[1].set_title('Residuals');
```
We can already see that the model fits the data much better. Also, the residuals are equally distributed and not dependent on X.

## Interpretation

To interpret the model coefficients, we first have to print them.

```{code-cell}
print(quadratic_model.summary())
```
### Estimates

We get three coefficients: The intercept, a linear and a quadratic coefficient. The linear regression coefficient ($\beta = 66.43$) is positive and significant. The quadratic regression coefficient is also significant but negative ($\beta = -5.17$). This means that the `Grade` (Y) first increases to a certain value of `Learn` (X) and then decreases.

### Model fit

With the quadratic predictor added, the model can explain 97.1% of the variance in Interest. Suggesting really good fit. Note that the negative quadratic effect can be visualized by an inverted U-shaped curve. To further evaluate the model, lets also look at the model fit from the linear model.

```{code-cell}
print(linear_model.summary())
```

The quadratic model only explains 82.1% of the variance (see `R-squared`). Also the AIC and BIC are much higher, also suggesting worse fit (compared to the quadratic model).