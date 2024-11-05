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

# 12.2 Centered Predictors

## Polynomial Regression and Meaningful Zero
1. Polynomial Regression: In polynomial regression, you’re not just fitting a linear term (like x) but also higher-order terms like $x^2, x^3$, etc.
2. Meaningful Zero: A “meaningful zero” in a predictor variable means that the zero point of this variable has a substantive interpretation. For instance, in a study measuring the effect of temperature on an outcome, 0°C is a meaningful zero as it represents the freezing point of water. However, not all variables have such interpretable zero points.
## Why Centering is Important
1. Interpretation of Lower Order Coefficients: In a higher-order polynomial equation, the coefficients of lower-order terms (like the linear x term in a quadratic or cubic equation) can be influenced by the inclusion of higher-order terms. This makes it challenging to interpret these coefficients independently because they are dependent on the specific values of the predictor variable.
2. Centering the Predictor: Centering involves subtracting the mean of the predictor variable from each data point. This shifts the data so that the mean becomes zero.
## Benefits of Centering
1. Simplifies Interpretation: When you center the predictor, the interpretation of lower-order terms becomes simpler. For example, in a centered quadratic model, the linear coefficient now tells you the rate of change at the mean of the predictor, rather than at zero, which might not be a meaningful or sensible point.
2. Reduces Multicollinearity: Centering can reduce multicollinearity between the predictor variables (e.g., x and x^2), making the model more stable and improving the accuracy of the estimated coefficients.

## Example
Consider a quadratic model: $\hat{y} = \beta_0 + \beta_1 \cdot x + \beta_2 \cdot x^2$

- Without centering, β1 (the coefficient of x) is interpreted as the rate of change of y with respect to x when x is zero. If zero is not meaningful (e.g., if x is years of experience), this interpretation doesn’t make much practical sense.
- With centering (say x is now (x - mean(x))), β1 is interpreted as the rate of change of y with respect to x when x is at its mean. This often provides a more meaningful and interpretable coefficient.

In summary, centering predictors in polynomial regression helps make the coefficients of lower-order terms more interpretable by ensuring that they reflect changes around a meaningful point (typically the mean) rather than an arbitrary zero point.

Therefore, in the current example, we should center the predictor `learn` in order to achieve a meaningful interpretation of the regression coefficients.

## Centering in Python

To center our variables, we subtract the mean from our predictor.

```{code-cell}
# Load packages
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
import statsmodels.api as sm
from sklearn.preprocessing import PolynomialFeatures

# Simulate dataset (again)
np.random.seed(69)  # For reproducibility
learn = np.linspace(0, 13, 500)
grade = -5 * (learn - 6.5)**2 + 100 + np.random.normal(0, 10, learn.shape)
data = pd.DataFrame({'learn': learn, 'grade': grade})
data = data[data['learn'] <= 8]

# Center predictor
data['learn_centered'] = data['learn'] - data['learn'].mean()

# You can ignore this
learn = np.array(data['learn_centered'])
grade = np.array(data['grade'])
```

Lets refit and plot our quadratic model.

```{code-cell}
polynomial_features_p2 = PolynomialFeatures(degree=2, include_bias=True)
learn_p2 = polynomial_features_p2.fit_transform(learn.reshape(-1, 1))

quadratic_model = sm.OLS(grade, learn_p2).fit()
quadratic_fit = quadratic_model.predict(learn_p2)
quadratic_residuals = quadratic_model.resid

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

# Print Summary
print(quadratic_model.summary())
```

### Interpretation

1. The expected `grade` (Y) for average `learn` (X) equals 69.26.
2. The linear regression of Y on X at the point x=0 (the mean of X) equals to 25.03. The positive coefficient tells us that at the mean of X, the criterion Y is still increasing. This value also indicates the average linear slope of the regression of Y on X in the quadratic equation.
3. The negative quadratic coefficient tells us that the function is inverted U-shaped.

Note that the total explained variance of Y by all the predictors does not change before and after centering.