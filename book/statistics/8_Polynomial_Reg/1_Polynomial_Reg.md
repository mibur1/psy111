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

# 8.1 Polynomial Regression in Python

To compute an Polynomial Regression in Python we will use the `statsmodels` package and the `sklearn` package.

## Example dataset

To demostrate polynomial regression, lets simulate a dataset that contain data from the introductory example. It contains two variables:

- `learn` - Hours learned per day
- `grade` - Exam grade (from -100 to 100)

```{code-cell}
# Load packages
import numpy as np
import pandas as pd
import seaborn as sns
import statsmodels.api as sm
import matplotlib.pyplot as plt
from sklearn.preprocessing import PolynomialFeatures

# Simulate dataset
np.random.seed(69)  # For reproducibility
learn = np.linspace(0, 13, 500)
grade = -5 * (learn - 6.5)**2 + 100 + np.random.normal(0, 10, learn.shape)
data = pd.DataFrame({'learn': learn, 'grade': grade})
data = data[data['learn'] <= 8]
learn = np.array(data['learn'])
grade = np.array(data['grade'])

# Inspect dataset
print(data)
```

Lets plot the relationship between the variables.

```{code-cell}
plt.figure(figsize=(14, 8))
# Scatter plot of the original data
plt.subplot(2, 1, 1)
sns.scatterplot(data=data, x='learn', y='grade', alpha=0.6)
plt.title('Scatter Plot: Learn vs. Grade')
plt.xlabel('Learn [h]')
plt.ylabel('Grade [%]')
plt.ylim(0, 110)
plt.xlim(0, 10)
plt.grid(True)
```
One can already see that there is a non-linear component present. Lets still fit a linear function and inspect the residuals.

## Detecting curvilinear relations

### Fit a linear model

To begin with, lets fit a simple linear model. Note that we are already using the function which we will later use to fit higher-order polynomials. Here we set the order to 1. Note that a polynomial with the order 1 is actually a linear model.

```{code-cell}
polynomial_features_p1 = PolynomialFeatures(degree=1, include_bias=True)
learn_p1 = polynomial_features_p1.fit_transform(learn.reshape(-1, 1))

linear_model = sm.OLS(grade, learn_p1).fit()
linear_fit = linear_model.predict(learn_p1)
linear_residuals = linear_model.resid
```

Next, lets plot the model and the model residuals.


```{code-cell}
plt.figure(figsize=(14, 6))

# Plot the model
plt.subplot(1, 2, 1)
sns.scatterplot(x=learn, y=grade, label='Actual Data', color='blue')
plt.plot(learn, linear_fit, color='red', label='Fitted Line', linewidth=2)
plt.title('Linear Regression: Learn vs. Grade')
plt.xlabel('Learn')
plt.ylabel('Grade')
plt.legend()
plt.grid(True)

# Plot the residuals
plt.subplot(1, 2, 2)
sns.scatterplot(x=learn, y=linear_residuals, color='red', label='Residuals')
plt.axhline(0, color='blue', linestyle='--', label='Zero Residual Line')
plt.title('Linear Regression Residuals')
plt.xlabel('Learn')
plt.ylabel('Residuals')
plt.grid(True)
plt.legend()

plt.tight_layout()
plt.show()
```

From the upper plot one can already see that whilst there being a linear trend present in the data, the model underestimates the complexitiy of the relationship. Looking at the residuals, it becomes clear that once the strong positive linear trend in the data has been removed, the curvilinearity stands out! The residuals are systematically related to the value of X: below zero for low and high values of X, and above zero for moderate values of X. This is the graphical diagnosis for the existence of a non-linear relationship, higher than degree “1”.

## Fit a polynomial regression

To improve model fit, lets inlcude higher order polynomials. To begin with, lets include a quadratic coefficient, making the polynomial a second order one. First, we use the `sklearn` package to generate the polynomial features.

```{code-cell}
polynomial_features_p2 = PolynomialFeatures(degree=2, include_bias=True)
learn_p2 = polynomial_features_p2.fit_transform(learn.reshape(-1, 1))
```

Next, fit the new model and extract its residuals.

```{code-cell}
quadratic_model = sm.OLS(grade, learn_p2).fit()
quadratic_fit = quadratic_model.predict(learn_p2)
quadratic_residuals = quadratic_model.resid
```

Lets also plot our new model and its residuals.

```{code-cell}
plt.figure(figsize=(14, 6))

# Plot the model
plt.subplot(1, 2, 1)
sns.scatterplot(x=learn, y=grade, label='Actual Data', color='blue')
plt.plot(learn, quadratic_fit, color='green', label='Quadratic Fit', linewidth=2)
plt.title('Quadratic Regression: Learn vs. Grade')
plt.xlabel('Learn')
plt.ylabel('Grade')
plt.legend()
plt.grid(True)

# Plot the residuals
plt.subplot(1, 2, 2)
sns.scatterplot(x=learn, y=quadratic_residuals, color='orange', label='Residuals')
plt.axhline(0, color='green', linestyle='--', label='Zero Residual Line')
plt.title('Quadratic Regression Residuals')
plt.xlabel('Learn')
plt.ylabel('Residuals')
plt.legend()
plt.grid(True)

plt.tight_layout()
plt.show()
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