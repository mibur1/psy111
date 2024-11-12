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

# 9.2 Regression Splines

To compute Regression Splines in Python we will again use the `patsy` package and the `statsmodels` package. Also, we continue using the `Wage` dataset. 

```{code-cell}
# Load packages and dataset
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
from patsy import dmatrix
import statsmodels.api as sm
from ISLP import load_data
df = load_data('Wage')
```

## Fit a first-order polynomial

Similar to fitting a stepwise function, we first need to transform our predictor variable `age`. We use the `patsy` (loaded as `dmatrix`) to create the corresponding data matrix. 

This time we know two knots (40 and 60) and first-order polynomial.

### Preparation

```{code-cell}
# Transform predictor variable
transformed_x = dmatrix("bs(age, knots=(40,60), degree=1, include_intercept=False)",
                        {"age": df['age']}, return_type='dataframe')
```

Notice `degree = 1` and `knots=(40,60)`.

### Fit the model

```{code-cell}
# Fit the model
fir_deg_model = sm.GLM(df['wage'], transformed_x).fit()

# Print the model summary
print(fir_deg_model.summary())
```

The `coeff` column indicates the coefficients of `wage` (Y) regressing on each basis function of `age` (X), i.e. $b_0, b_1, b_2, b_3$. 

Note, spline regression coefficients are not analogous to slope coefficients in simple linear regression. i.e., they are not directly interpretable as increase or decrease on Y given one-unit increase on X. Spline regression coefficients scale the computed basis functions for a given value of X (see slides from the multivariate statistics lecture).


### Plot the model

```{code-cell}
# Plot the model
plt.figure(figsize=(10, 6))
xp = np.linspace(df['age'].min(), df['age'].max(), 100)
transformed_xp = dmatrix("bs(xp, knots=(40,60), degree=1, include_intercept=False)",
                         {"xp": xp}, return_type='dataframe')

pred = fir_deg_model.predict(transformed_xp)

sns.scatterplot(x=df['age'], y=df['wage'], alpha=0.5, label='Data')
plt.plot(xp, pred, label='linear fit', color='red')
plt.title("Linear (first-order) Fit")
plt.xlabel("Age")
plt.ylabel("Wage")
plt.legend().remove()
plt.show()
```

The model plot shows that we have gone from fitting an intercept (i.e., a horizontal line) in each bin to fitting a linear function in each bin. Note that we also had to change the value when computing `xp_transformed` (`knots=(40,60), degree=1`).