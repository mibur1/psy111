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

# 13.2 Regression Splines

To fit regression splines, we continue with the same data as in the previous section. Again, we want to predict `wage` from `age` in the Mid-Atlantic Wage Dataset.

```{code-cell}
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
import patsy
import statsmodels.api as sm
from ISLP import load_data

# Load the data
df = load_data('Wage')
```

## Fitting piecewise linear splines

Similar to fitting a stepwise function, we first need to create the design matrix for our transformed predictor `age`. This time we specify two knots (at age 40 and 60), and first-order polynomial:

```{code-cell}

transformed_age = patsy.dmatrix("bs(age, knots=(40,60), degree=1)",
                                data={"age": df['age']},
                                return_type='dataframe')
```

We then create and fit the model:

```{code-cell}
# Fit the model
model = sm.OLS(df['wage'], transformed_age)
model_fit = model.fit()

print(model_fit.summary())
```

The `coeff` column indicates the coefficients of `wage` regressing on each basis function of `age`, i.e. $b_0, b_1, b_2, b_3$. 

However, spline regression coefficients are not analogous to slope coefficients in simple linear regression and they are not directly interpretable as increase or decrease on Y given one-unit increase on X! Spline regression coefficients scale the computed basis functions for a given value of X (see slides from the multivariate statistics lecture).


## Plotting the model

```{code-cell}
# Create evenly spaced values to plot the model predictions
xp = np.linspace(df['age'].min(), df['age'].max(), 100)
xp_trans = patsy.dmatrix("bs(xp, knots=(40,60), degree=1)",
                         data={"xp": xp},
                         return_type='dataframe')

predictions = model_fit.predict(xp_trans)

# Plot the model
sns.scatterplot(data=df, x="age", y="wage", alpha=0.4)
plt.plot(xp, predictions, color='red')
plt.title("Linear (first-order) spline regression");
```

The model plot shows that we have gone from fitting an intercept (i.e., a horizontal line) in each bin to fitting a linear function in each bin.