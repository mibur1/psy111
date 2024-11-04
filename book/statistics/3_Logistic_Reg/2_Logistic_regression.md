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

# 3.3 Logistic Regression

Next, we will use logistic regression, which naturally keeps predicted probabilities between 0 and 1, making it more suitable for our research question.

```{code-cell}
#import libraries
import matplotlib.pyplot as plt
import numpy as np
from sklearn.linear_model import LogisticRegression
from sklearn.metrics import classification_report, confusion_matrix
import pandas as pd

#importing the dataset
#\t because columns would be interpreted wrong
data = pd.read_csv("data/data.dat", delimiter='\t')

#reshape the data
X = data['age'].values.reshape(-1, 1)
y=data['display']

#logistic regression
model= LogisticRegression().fit(X, y)
#intercept
intercept= model.intercept_
#coefficients
coef= model.coef_
#print intercept and coefficient
print(f"Intercept: {intercept}")
print(f"Coefficients: {coef}")
```
**Interpretation**
- *intercept*=-2,83: This means that when age = 0, the expected logit (log-odds) of the chance of understanding display rules is -2.83.
- *coef*= 0,066 (coefficient for age) : This indicates that for each one-month increase in age, the log-odds of understanding display rules increases by 0.066.

We can now have a look at the plot.

```{code-cell}
# Generate predictions for the range of ages
x_range = np.linspace(X.min(), X.max(), 100).reshape(-1, 1)

# calculate the predictions for positives
y_prob = model.predict_proba(x_range)[:, 1]  

# Plot the results
plt.figure(figsize=(10, 6))

# Scatter plot of the actual data
plt.scatter(X, y, color='blue', label='data points', alpha=0.5)

# Plot the logistic regression
plt.plot(x_range, y_prob, color='red', label='Logistic Regression')

# labels and titles
plt.xlabel('age')
plt.ylabel('display')
plt.title('Logistisc Regression: age vs display')
plt.legend()

# Show the plot
plt.show()
```
## Evaluation of the model
To evaluate our model, we can examine how many values of $Y$ (understanding display rules) were predicted correctly by the model.

```{code-cell}

#model.predict_proba(X)
print(model.predict(X))
#evaluate the model
print(f"Predicted accuarcy:{model.score(X, y)}")

```
The first array shows the predicted output, with 77.1% accuracy. For a more detailed evaluation, we will use the confusion matrix. 

**Confusion matrix**

```{code-cell}

#confusion matrix
print(f"Confusion matrix: {confusion_matrix(y, model.predict(X))}")

```

The output of the confusion matrix provides the following values:

- True negatives in the upper-left position
- False negatives in the lower-left position
- False positives in the upper-right position
- True positives in the lower-right position

It’s helpful to visualize these values in a plot. You can use the following approach:

```{code-cell}

cm = confusion_matrix(y, model.predict(X))

fig, ax = plt.subplots(figsize=(8, 8))
ax.imshow(cm)
ax.grid(False)
ax.xaxis.set(ticks=(0, 1), ticklabels=('Predicted 0s', 'Predicted 1s'))
ax.yaxis.set(ticks=(0, 1), ticklabels=('Actual 0s', 'Actual 1s'))
ax.set_ylim(1.5, -0.5)
for i in range(2):
    for j in range(2):
        ax.text(j, i, cm[i, j], ha='center', va='center', color='red')
plt.show()
```
**Classification report**
For a deeper inspection of the model's accuracy, we can print the classification report.

```{code-cell}
from sklearn.metrics import classification_report

# Generate classification report
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



## Multiple Regression
You may want to use two or more variables as inputs for the regression. In our example, we will use `age` and `TOM` as predictors for `display` by simply adding them to $X$.


```{code-cell}
X = data[['age', 'TOM']]  # Using both 'age' and 'TOM' as predictors
y = data['display']  # Target variable

# Fit the logistic regression model
logr = LogisticRegression().fit(X, y)

# Get intercept and coefficients
intercept = logr.intercept_
coef = logr.coef_

print(f"Intercept: {intercept}")
print(f"Coefficients: {coef}")
```
