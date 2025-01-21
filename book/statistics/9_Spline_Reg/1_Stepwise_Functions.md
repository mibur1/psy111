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

## The Dataset

We will use the Mid-Atlantic Wage Dataset from the [ISLP library](https://islp.readthedocs.io/en/latest/) to showcase fitting stepwise functions. Our research goal is to predict `wage` for different `age` ranges by taking the average wage within each bin as our estimate for prediction.

```{code-cell}
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
import statsmodels.api as sm
import patsy
from ISLP import load_data

# Load and plot the data
df = load_data('Wage')

sns.scatterplot(x=df['age'], y=df['wage'], alpha=0.4)
plt.title("ISLP data: Wage distribution across age");
```

## Fitting a stepwise function

To fit a stepwise funtion, we first need to get cut points for our predictor variable `age`. For example, we can decide to split our data into 4 equal parts:

```{code-cell}
bins = pd.cut(df['age'], 4)
bins
```

The output provides us with intervals for evenly sized bins of `age`. We can use the upper value of each interval to get cut points for our stepwise function. We then use the `dmatrix` function to create a design matrix:

```{code-cell}
transformed_age = patsy.dmatrix("bs(age, knots=(33.5, 49, 64.5), degree=0)",
                                data={"age": df['age']},
                                return_type='dataframe')
```

When you specify `"bs(age, ...)"`, you tell `dmatrix` to transform age using B-spline basis functions. These basis functions segment the variable age into piecewise polynomials defined by the specified knots and degree. When you set degree zo zero, it creates piecewise constant functions, meaning the resulting function is a step function consisting of only horizontal segments.

### Fitting the model

Once we have the age in our design matrix, we can fit the model like a normal categorical regression model:

```{code-cell}
model = sm.OLS(df['wage'], transformed_age)
model_fit = model.fit()

print(model_fit.summary())
```

The summary tells us:

- The average wage in bin 1 within the age range from 17.9 to 33.5 years equals
to $94.16k per year.
- The wage difference in bin 2 (i.e., between 33.5 and 49 years) as compared with
bin 1 equals to $23.93k per year.
- The wage difference in bin 3 (i.e., between 49 and 64.5 years) as compared with bin 1 equals to $23.89k per year.
- The wage difference in bin 4 (i.e., between 64.5 and 80.1 years) as compared with bin 1 equals to $7.64k per year.

The second and the third bin differ significantly from the first bin. The third bin does not differ significantly from the first one (p = .126)

In summary, the model suggests that wages vary with age, but the relationship is not the same across all age groups. Wages appear to increase with age up to a point, but the increase is not statistically significant for the oldest age group in this sample. 

### Plotting the model


We can also plot the model. Note that bins 2 and 3 have very similar estimates, which leads to minimal differentiation in wage trends between these groups. 

```{code-cell}
# Plot the model
plt.figure(figsize=(10, 6))
xp = np.linspace(df['age'].min(), df['age'].max(), 100)
xp_trans = patsy.dmatrix("bs(xp, knots=(33.5, 49, 64.5), degree=0)",
                         data={"xp": xp},
                         return_type='dataframe')

predictions = model_fit.predict(xp_trans)

sns.scatterplot(data=df, x="age", y="wage", alpha=0.4)
plt.plot(xp, predictions, color='red')
plt.title("ISLP data: stepwise fit (zero-order)");
```