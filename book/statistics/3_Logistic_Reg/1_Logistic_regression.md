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

Before using logistic regression to model our data, we will attempt to do so through simple linear regression. While linear regression is not suitable for dichotomous outcomes, visualizing it can help illustrate why logistic regression is a better fit for our research question.

## Why Not Linear Regression?

```{code-cell}
import pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt

df = pd.read_csv("data/data.dat", delimiter='\t')
sns.regplot(x="age", y="display", data=df)
plt.xlabel("Age [month]")
plt.ylabel("Understanding display rules")
plt.show()
```

As you can see, linear regression struggles with binary outcomes, as evidenced by predicted values exceeding 1 beyond approximately 80 months, which is invalid for probabilities. Since our dependent variable is dichotomous (e.g., pass/fail), we need a model that restricts predicted values to fall between 0 and 1, such as logistic regression.


## Logistic Regression

Logistic regression naturally ensures that predicted probabilities stay between 0 and 1. In this tutorial, we will use the `LogisticRegression()` class from `scikit-learn` for modeling.

```{code-cell}
import numpy as np
from sklearn.linear_model import LogisticRegression

# convert 'age' into a NumPy array and reshape it to a 2D array (required for the model)
# .reshape(-1, 1): Creates one column with as many rows as needed (-1 infers the row count)
X = np.asarray(df['age']).reshape(-1, 1) 
# convert 'display' to a NumPy array for the binary outcome
y = np.asarray(df['display']) # binary outcome

model = LogisticRegression()
results = model.fit(X, y)

print(f"Intercept: {results.intercept_}")
print(f"Coefficients: {results.coef_}")
```

### Interpreting the Model Outputs

- **Intercept (-2.83):** The expected logit (log-odds) of the outcome (understanding display rules) when age = 0.
- **Coefficient (0.066):** For each one-month increase in age, the log-odds of understanding display rules increases by 0.066.

The output of a logistic regression model is linear in the log-odds (logits). Each coefficient in the logistic regression tells us how a one-unit change in a predictor affects the log-odds of the outcome. While logits aren't as intuitive as probabilities, they allow the model to establish a linear connection between the predictors and the outcome. This linearity makes it easier to interpret and estimate relationships mathematically.


### From Logits to Probabilities

We can simply transform the logits into probabilities (more specifically, the conditional probability of Y belongig to class 1 given X):

$$P(Y=1 \mid X) = \frac{1}{1 + e^{-(b_0 + b_1 X)}}$$

To better understand the model's behavior, let’s plot its outputs. A simple way to do this is by ceating an evenly spaced array of values for our range, and then use `model.predict()` to predict the outcome for each value. This will generate the regression line:

```{code-cell}
# create an evenly spaced array of values for the range 
x_range = np.linspace(1, 100, 100).reshape(-1, 1) 
# predict probability of class 1 for each value in the range
y_prob = model.predict_proba(x_range)[:, 1] 

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
# Accuracy: Ratio of correct predictors to the toal number of predictors. 
print("\nAccuracy:", model.score(X, y))
```
An accuracy of 77% indicates the that the model correctly predicts the outcome for about 77% of the children in our data. This suggests that the model peforms reasonably well, altough it may still misclassify some cases.

For further investigation, a confusion matrix is a useful way to visualize the prediction accuracy.

```{code-cell}
from sklearn.metrics import confusion_matrix, classification_report
print(f"Confusion matrix:\n {confusion_matrix(y, model.predict(X))}")
```

The output of the confusion matrix provides the following values:

|   True negative   |   False negative  |
|-------------------|-------------------|
|   False positive  |   True positive   |

*Remember*:
- **True negatives**: The model correctly predicts the absence of the outcome (e.g., correctly predicts "fail").
- **False negatives**: The model incorrectly predicts the absence of the outcome (e.g., wrongly predicts "fail" when the child actually passes).
- **False positives**: The model incorrectly predicts the presence of the outcome (e.g., wrongly predicts "pass" when the child fails).
- **True positives**: The model correctly predicts the presence of the outcome (e.g., correctly predicts "pass").


For an even deeper inspection of the model's accuracy, we can print the classification report:

```{code-cell}
report = classification_report(y, model.predict(X))
print(report)

```
The output can be interpreted as follows:

**Precision**: Propportion of true positive predictions among all positive predictions made by the model.

$$\text{Precision} = \frac{\text{True Positives (TP)}}{\text{True Positives (TP)} + \text{False Positives (FP)}}$$

- *Class 0: 0.73 precision indicates that when the model predicts a "fail", 73% of the time it is correct*
- *Class 1: 0.81 precision means that when the model predicts "pass," 81% of the time it is correct* 


**Recall**: The proportion of true positives that are correctly identified by the model.

$$\text{Recall} = \frac{\text{True Positives (TP)}}{\text{True Positives (TP)} + \text{False Negatives (FN)}}$$

- *Class 0: 77% of the actual "fails" are correctly identified by the model.*
- *Class 1: 77% of the actual "passes" are correctly identified by the model.* 


**F1-Score**: harmonic mean of precision and recall, providing a balance between the two and offering a good overall measure of model performance.

$$F_1 = 2 \cdot \frac{\text{Precision} \cdot \text{Recall}}{\text{Precision} + \text{Recall}}$$

- *For class 0, it is 0.75 and for class 1, it is 0.79. This suggests the model is sligthly more effective at correctly predicting class 1.*

**Support**: actual occurence of each class in the dataset

**Accuracy**: The overall proportion of correctly predicted observations.

$$\text{Accuracy} = \frac{TP + TN}{\text{Total number of observations}}$$
- *model correctly predicts the outcome 77% of the time, which is fairly good*


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
