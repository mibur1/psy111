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

# 7.2 The Logit Transform

$$ln(\frac{P(Y=1 \mid X)}{1 - P(Y=1 \mid X)}) = b_0 + b_1 \cdot X$$

By default, logistic regression applies the logit transform to convert probabilities into a log scale, which linearizes the relationship between the independent variable and the probability of a binary outcome (like success/failure). 

In this formula:
- $b_0$ is the intercept, representing the logit value when $X = 0$.
- $b_1$ is the slope, indicating how the logit changes with a one-unit increase in $X$.

This transformation is crucial because it allows us to apply linear regression techniques to binary outcomes. The plot of the logit against the independent variable, such as age, helps visualize how the likelihood of the outcome varies with age. This aids in both interpretation and understanding of the model. For further details, refer to the lecture slides, pages 40 to 45.

We can plot the logits to see, how the linear relationship looks like:

```{code-cell}
import pandas as pd
import matplotlib.pyplot as plt
from sklearn.linear_model import LogisticRegression

# Get the data
data = pd.read_csv("data/data.dat", delimiter='\t')
X = data[['age']]
y = data['display']

# Fit the model
model = LogisticRegression()
results = model.fit(X, y)

intercept = results.intercept_[0]
coef = results.coef_[0][0]

# Logit transformation: compute the logit for each age
data['logit'] = intercept + coef * data['age']

# Plot
fig, ax = plt.subplots()
ax.plot(data['age'], data['logit'], 'r-', label='Logit Transform')  # Logit line
ax.scatter(data['age'], data['logit'], color='blue', alpha=0.5, label='Data points')
ax.set(xlabel="Age", ylabel="Logit (Log-Odds)", title="Logit Transform")

plt.legend()
plt.show()
```

