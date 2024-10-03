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

# 5.1 Multiple Linear Regression

Multiple linear regression involves performing linear regression with more than one independent variable. As you may know, multiple regression with kk predictors can be expressed as:

$$y_i = \beta_0 + \beta_1 x_{i1} + \beta_2 x_{i2} + \dots + \beta_k x_{ik} + \epsilon_i$$

In this equation:

- $\epsilon_i$ represents the residual variance that is not explained by the model.
- $\beta_0$​ is the intercept, representing the expected value of $y$ when all $x$-values (predictors) are 0.
- $\beta_1$​ represents the change in $y$ for a one-unit increase in $x_i$​, while all other predictors are held constant.
- The same interpretation applies to the other predictors, $\beta_2, \beta_3, ..., \beta_k$


```{admonition} Independent and dependent variables
:class: note
- **Dependent variable**: The variable we are trying to explain with our model (outcome)
- **Independent variables**: The variable we use to explain the dependent variable (predictors)
```

In Python, various methods and libraries are available for performing multiple regression. Some methods involve manual implementation, while others utilize libraries such as `sklearn` or `statsmodels`. For this example, we will focus on using `statsmodels`.

We will work with a dataset called trees from the `datasets` package, which includes measurements of the girth, height, and volume of 31 felled black cherry trees.


## Step 1: Import the Required Libraries

```{code-cell}
import pandas as pd
import statsmodels.api as sm
import statsmodels.formula.api as smf
import seaborn as sns
import numpy as np
import matplotlib.pyplot as plt
```

## Step 2: Load and Preprocess the Dataset

We will now import the "trees" dataset and convert the measurements from inches, feet, and cubic feet to meters and cubic meters. After that, we’ll view the first few rows of the dataset using the `head()` method.

```{code-cell}
trees = sm.datasets.get_rdataset('trees').data
df = trees
df['Girth'] = df['Girth'] * 0.0254
df['Height'] = df['Height'] * 0.3048
df['Volume'] = df['Volume'] * 0.0283168
df =df.round(3)
print(df.head())
```

## Step 3: Visualize the Data

Before we proceed with modeling, it is helpful to visualize the relationships between the variables. This can guide us in determining whether linear regression is appropriate. We’ll use the `pairplot()` function from the `seaborn`package to create scatterplot matrices.

```{code-cell}
sns.pairplot(df);
plt.show()
```

## Step 4: Fit the Multiple Linear Regression Model

Based on the visual inspection, there is a strong linear relationship between Volume and Girth, and a weaker one between Volume and Height. Now, we will create a linear regression model with `Volume` as the dependent variable and `Girth` and `Height` as the independent variables.

We’ll use the `ols()` function from `statsmodels` to build the model. The formula is specified in an R-style format: `response ~ predictor(s)`. In this case, the independent variables are separated by a `+` sign. The `summary()` function provides a detailed overview of the model results.


```{admonition} Ordinary Least Squares (OLS)
:class: note
OLS is a method used to minimize the sum of squared differences between the observed values of the dependent variable and the predicted values from the model.
```

```{code-cell}
model = smf.ols(formula='Volume ~ Girth + Height', data=df).fit()
print(model.summary())
```

## Step 5: Interpret the Regression Summary

The regression summary provides important information about the model:

- **Intercept** ($\beta_0$​): This is the predicted value of `Volume` when `Girth` and `Height` are both zero.
- **Girth** ($\beta_1$​): This represents the change in `Volume` for a one-unit increase in `Girth`, assuming Height remains constant.
- **Height** ($\beta_2$): This represents the change in `Volume` for a one-unit increase in `Height`, assuming Girth remains constant.

Each coefficient includes:

- **Standard error**: Measures the accuracy of the coefficient estimate.
- **t-value**: Tests the hypothesis that the coefficient is different from zero.
- **p-value**: A small p-value suggests that the predictor variable is statistically significant.

R-squared and Adjusted R-squared

- R-squared: Indicates the proportion of variance in the dependent variable explained by the model. It increases with more predictors, even if they don't improve the model.
- Adjusted R-squared: Adjusts for the number of predictors, penalizing for adding unnecessary predictors, and is more reliable for evaluating model performance.

F-statistic

- The F-statistic compares the fit of the model to a model with no predictors. A large F-statistic and a low p-value indicate that the independent variables have real predictive power.

## Step 6: Make Predictions

Now that we have a fitted model, we can make predictions using new values for `Girth` and `Height`. To do this, we use the `predict()` method.

```{code-cell}
X_predict = pd.DataFrame({'Girth': [0.3, 0.4, 0.5],
                   'Height': [20, 21, 22]})
prediction_new = model.get_prediction(X_predict)
print(prediction_new.predicted_mean)
```

The output shows the predicted tree volumes for the provided values of `Girth` and `Height`.

```{admonition} Summary
:class: tip
- `sns.pairplot()` from `seaborn` is useful for scatterplot matrices.
- The `ols()` function from `statsmodels` can be used to fit multiple linear regression models.
```