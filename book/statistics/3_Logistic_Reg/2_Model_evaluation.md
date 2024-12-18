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

# 7.2 Model Evaluation

To evaluate our model, we can examine how many values of $y$ (understanding display rules) were predicted correctly by the model:

```{code-cell}
import numpy as np
import pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt
from sklearn.linear_model import LogisticRegression

df = pd.read_csv("data/data.dat", delimiter='\t')
X = np.asarray(df['age']).reshape(-1, 1) 
y = np.asarray(df['display']) # binary outcome

model = LogisticRegression()
results = model.fit(X, y)

predictions = model.predict(X)
accuracy =  model.score(X, y)

print("Model predictions:", predictions)
print("\nAccuracy:", accuracy) 
```

An accuracy of 77% indicates the that the model correctly predicts the outcome for about 77% of the children in our data. This suggests that the model peforms reasonably well, altough it still misclassifies some cases. For a more detailed investigation, a confusion matrix is a useful way to visualize the prediction accuracy:

```{code-cell}
from sklearn.metrics import confusion_matrix, classification_report
print(f"Confusion matrix:\n {confusion_matrix(y, model.predict(X))}")
```

The output of the confusion matrix provides the following values:

|                     | Predicted Negative  | Predicted Positive  |
|---------------------|-------------------- |---------------------|
| **Actual Negative** | True Negative (TN)  | False Positive (FP) |
| **Actual Positive** | False Negative (FN) | True Positive (TP)  |

For an even deeper inspection of the model's accuracy, we can print the classification report:

```{code-cell}
report = classification_report(y, model.predict(X))
print(report)

```
The output can be interpreted as follows:

**Precision**: Propportion of true positive predictions among all positive predictions made by the model.

$$\text{Precision} = \frac{\text{True Positives (TP)}}{\text{True Positives (TP)} + \text{False Positives (FP)}}$$

- *Class 0: When the model predicts that a sample does not understand the display rules (Class 0), 73% of the time it is correct.*
- *Class 1: When the model predicts that a sample does understand the display rules (Class 1), 81% of the time it is correct. * 


**Recall**: The proportion of true positives that are correctly identified by the model.

$$\text{Recall} = \frac{\text{True Positives (TP)}}{\text{True Positives (TP)} + \text{False Negatives (FN)}}$$

- *Class 0:  77% of the actual samples that do not understand the display rules (Class 0) are correctly identified by the model.*
- *Class 1: 77% of the actual samples that do understand the display rules (Class 1) are correctly identified by the model.* 


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
