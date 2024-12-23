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

# 13.1 Stepwise Functions

To compute Stepwise Functions in Python we will use the `patsy` package and the `statsmodels` package. 

## Example dataset

We use the `Wage` dataset to showcase fitting stepwise functions. Our research goal is: Predict variation of `wage` (Y) in different `age` (X) ranges by taking the average wage within the given X bin as best estimate for prediction.

The following code chunk load and plots our variables of interest.

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

# Plot the data
plt.figure(figsize=(10, 6))
sns.scatterplot(x=df['age'], y=df['wage'], alpha=0.5, label='Data')
plt.title("Wage by Age Scatterplot")
plt.xlabel("Age")
plt.ylabel("Wage")
plt.legend().remove()
plt.show()
```

## Fit a stepwise function

To fit a stepwise funtion we first have to transform our variables.

### Preparation

To fit a stepwise function we first have to transform our predictor variable `age`.

```{code-cell}
# Get cut points
bins = pd.cut(df['age'], 4)
print(bins) 
```

The output provides us with cut points for evenly sized bins of `age`. Lets use these cutpoint to transform our predictor variable to fit a stepwise function.

```{code-cell}
# Transform predictor variable
transformed_x = dmatrix("bs(age, knots=(33.5, 49, 64.5), degree=0, include_intercept=False)",
                        {"age": df['age']}, return_type='dataframe')
```

Note two things:

1. We want to fit stepwise functions, i.e. a constant for each bin. This is equal to fit a zero-degree polynomial (i.e. a function with an intercept an no further parameters).
2. We used the cut points suggested by the `pd.cut()` output. For 4 bins with need to specifiy 4 cut points.

### Fit the model

To fit the model we use the `statsmodels` function `GLM`. Let's call it `zero_deg_model` refering to zero-order polynomial.

```{code-cell}
# Fit the model
zero_deg_model = sm.GLM(df['wage'], transformed_x).fit()

# Print the model summary
print(zero_deg_model.summary())
```

This model is a categorical regression model, as we cut the age into four discrete category bins:

- The average wage in bin 1 within the age range from 17.9 to 33.5 years equals
to 94.16 thousand dollars per year.
- The wage difference in bin 2 (i.e., between 33.5 and 49 years) as compared with
bin 1 equals to 23.93 thousand dollars per year.
- The wage difference in bin 3 (i.e., between 49 and 64.5 years) as compared with bin 1 equals to 23.89 thousand dollars per year.
- The wage difference in bin 4 (i.e., between 64.5 and 80.1 years) as compared with bin 1 equals to 7.64 thousand dollars per year.

The second and the third bin differ significantly from the first bin. The third bin does not differ significantly from the first one (p = .126)

In summary, the model suggests that wages vary with age, but the relationship is not the same across all age groups. Wages appear to increase with age up to a point, but the increase is not statistically significant for the oldest age group in this sample. 

### Plot the model

```{code-cell}
# Plot the model
plt.figure(figsize=(10, 6))
xp = np.linspace(df['age'].min(), df['age'].max(), 100)
transformed_xp = dmatrix("bs(xp, knots=(33.5, 49, 64.5), degree=0, include_intercept=False)",
                         {"xp": xp}, return_type='dataframe')

pred = zero_deg_model.predict(transformed_xp)

sns.scatterplot(x=df['age'], y=df['wage'], alpha=0.5, label='Data')
plt.plot(xp, pred, label='stepwise fit', color='red')
plt.title("Stepwise (zero-order) Fit")
plt.xlabel("Age")
plt.ylabel("Wage")
plt.legend().remove()
plt.show()
```

The plot shows the fitted regression line. Altough it seems like we only have 3 bins, we actually have 4. However, as the second and the third bin have a very similar estimate (23.99 vs. 23.88) it seems like there a only 3 bins.