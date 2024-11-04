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

# 3.4 Logit Transfrom
To convert the probabilities from the previous output into a log scale, we apply the formula:

$$
\ln\left(\frac{\text{P(Y=1|X)}}{1-\text{P(Y=1|X)}}\right) = b_0 + b_1 \cdot X
$$

- $b_0$ as the intercept and the logit value when $X=0$
- $b_1$ as the slope, which indicates the change of a one-unit-increase of $X$

**Why would you transform the values?**

The output of a logistic regression model indicates a linear relationship between the independent variables (predictors) and the log-odds of the dependent variable. Each coefficient shows how a one-unit change in the corresponding predictor affects the log-odds of the outcome, while keeping other variables constant.

```{code-cell}
#import the libraries
import pandas as pd
import matplotlib.pyplot as plt
import numpy as np
from sklearn.linear_model import LogisticRegression

# Load the data
data = pd.read_csv("data/data.dat", delimiter='\t')

# Define X and y (age and target variable 'display')
X = data[['age']]
y = data['display']

# Fit the logistic regression model
logr = LogisticRegression()
logr.fit(X, y)

# Get intercept and coefficient
intercept = logr.intercept_[0]
coef = logr.coef_[0][0]

# Logit transformation: compute the linear combination (logit = intercept + coef * age)
data['logit'] = intercept + coef * data['age']

# Plot the logit against age
plt.figure(figsize=(10, 6))
plt.plot(data['age'], data['logit'], 'r-', label='Logit Transform')  # Logit line
plt.scatter(data['age'], data['logit'], color='blue', alpha=0.5, label='Data points')  # Scatter plot of age vs logit

# Add labels and title
plt.xlabel('Age')
plt.ylabel('Logit (Log-Odds)')
plt.title('Logistic Regression: Logit Transform (Age vs Log-Odds)')
plt.legend()

# Show the plot
plt.show()
```

