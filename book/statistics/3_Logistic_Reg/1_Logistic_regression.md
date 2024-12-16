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

# 7.1 Logistic Regression

Before using logistic regression to model our data, we will attempt to do so through simple linear regression. Although this is not suitable for dichotomous outcomes, visualising it helps to illustrate why logistic regression is a better fit for our research question.

## Why Not Linear Regression?

```{code-cell}
import pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt

df = pd.read_csv("data/data.dat", delimiter='\t')
sns.regplot(x="age", y="display", data=df);
```

As you can see, linear regression struggles with binary outcomes. Beyond approximately 80 months, predicted values exceed 1, which is not valid for probabilities. Since our dependent variable is dichotomous (e.g., pass/fail), we need a model that restricts predicted values to fall between 0 and 1, such as logistic regression.


## Logistic Regression

Logistic regression naturally ensures that predicted probabilities stay between 0 and 1. In this tutorial, we will use the `LogisticRegression()` class from scikit-learn for modeling.

```{code-cell}
import numpy as np
from sklearn.linear_model import LogisticRegression

X = np.asarray(df['age']).reshape(-1, 1) # reshape age into a 2D array
y = np.asarray(df['display']) # binary outcome

model = LogisticRegression()
results = model.fit(X, y)

print(f"Intercept: {results.intercept_}")
print(f"Coefficients: {results.coef_}")
```

### Interpreting the Model Outputs

- **Intercept (-2.83):** The expected logit (log-odds) of the outcome (understanding display rules) when age = 0.
- **Coefficient (0.066):** For each one-month increase in age, the log-odds of understanding display rules increases by 0.066.

The output of a logistic regression model is linear in the log-odds (logits). Logits provide a linear relationship between predictors and the dependent variable. Each coefficient reflects how a one-unit change in the predictor impacts the log-odds of the outcome, making it easier to interpret the model.


### From Logits to Probabilities

We can simply transform the logits into probabilities (more specifically, the conditional probability of Y belongig to class 1 given X):

$$P(Y=1 \mid X) = \frac{1}{1 + e^{-(b_0 + b_1 X)}}$$

Let's plot the model outputs to better understand whats going on. A simple way of doing so it to create an evenly spaced array of values for our range, and then use `model.predict()` to predict the outcome for each value (this will give us the regression line):

```{code-cell}
x_range = np.linspace(1, 100, 100).reshape(-1, 1) # generate X values
y_prob = model.predict_proba(x_range)[:, 1] # get model predictions

# Plot the results
fig, ax = plt.subplots()
ax.scatter(X, y, color='blue', label='data points', alpha=0.5)     # actual data
ax.plot(x_range, y_prob, color='red', label='model predictions') # regression model
ax.set(xlabel='age', ylabel='display', title='Logistic Regression Model')

plt.legend()
plt.show()
```

## Evaluation of the model

To evaluate our model, we can examine how many values of $y$ (understanding display rules) were predicted correctly by the model:

```{code-cell}
print("Model predictions:", model.predict(X))
print("\nAccuracy:", model.score(X, y))
```

A nice way of visualising predictions is a confusion matrix:

```{code-cell}
from sklearn.metrics import confusion_matrix, classification_report
print(f"Confusion matrix:\n {confusion_matrix(y, model.predict(X))}")
```

The output of the confusion matrix provides the following values:

- True negatives in the upper-left position
- False negatives in the lower-left position
- False positives in the upper-right position
- True positives in the lower-right position

For an even deeper inspection of the model's accuracy, we can print the classification report:

```{code-cell}
report = classification_report(y, model.predict(X))
print(report)

```
The output can be interpreted as follows:

- Precision: Propportion of true positive predictions among all positive predictions

$$\text{Precision} = \frac{\text{True Positives (TP)}}{\text{True Positives (TP)} + \text{False Positives (FP)}}$$

- Recall: The proportion of true positives that are correctly identified by the model.

$$\text{Recall} = \frac{\text{True Positives (TP)}}{\text{True Positives (TP)} + \text{False Negatives (FN)}}$$

- F1-Score: harmonic mean of precision and recall. It offers a good balance between those two measuremnets and is therefore a good overall measure of performance.

$$F_1 = 2 \cdot \frac{\text{Precision} \cdot \text{Recall}}{\text{Precision} + \text{Recall}}$$

- support: actual occurence of each class in the daatset
- Accuracy: The overall proportion of correctly predicted observations.

$$\text{Accuracy} = \frac{TP + TN}{\text{Total number of observations}}$$


## Multiple Logistic Regression
You may want to use two or more variables as inputs for the regression. In our example, we will use `age` and `TOM` as predictors for `display` by simply adding them to $X$.

```{code-cell}
X = df[['age', 'TOM']]
y = df['display']

model = LogisticRegression()
results = model.fit(X, y)
report = classification_report(y, model.predict(X))
print(report)
```
