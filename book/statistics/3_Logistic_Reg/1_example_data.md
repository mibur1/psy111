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

# 7.1 The example dataset

For logistic regression, we will use the `LogisticRegression()` function from the `sklearn` library. Although there are several other packages and functions available for logistic regression, this one is particularly convenient and widely used.

To demonstrate the advantage of logistic regression over linear regression for dichotomous dependent variables and quantitative independent variables, we will explore its application in this context. Unlike linear regression, logistic regression is specifically designed to handle binary outcomes, making it a more appropriate model for predicting probabilities of categorical events.
**Research Aim**: Using children's age and theory of mind ability to predict whether they understand display rules or not.

**Context**: In developmental cognitive psychology, theory of mind is defined as the ability to understand the intentions, beliefs, and emotions of others. Display rules refer to social or cultural norms that guide how individuals should express their emotions in different contexts.

To address this research question, children were given a false belief task (a task commonly used to measure their ability to understand others' intentions) and a display rules understanding task, which they could either pass or fail (a dichotomous outcome). Their age in months was also recorded.

In total, 70 children, aged between 24 and 83 months, participated in the study.

## Linear regression
First, we will import the necessary libraries and then attempt to plot the data using linear regression. Although linear regression is not suitable for dichotomous outcomes, plotting it first helps illustrate why logistic regression is a better fit for our research question.

```{code-cell}
import pandas as pd
#\t because columns would be interpreted wrong
data = pd.read_csv("data/data.dat", delimiter='\t')
print(data)

```

```{code-cell}
#plot the data using seaborn

import seaborn as sns
import matplotlib.pyplot as plt

#reshape the data
X = data['age'].values.reshape(-1, 1)
y=data['display']

#linear regression
sns.regplot(x=X, y=y, line_kws={"color": "red"}, scatter_kws={"color": "blue"})

# Add labels and title
plt.xlabel('age')
plt.ylabel('display')
plt.title('Scatter Plot with Linear Regression Line')

# Show the plot
plt.show()

```
As you can see, linear regression is not a suitable model for our research question. Around 80 months, the predicted values of Y exceed 1, which doesn't make sense in this context. Since our dependent variable is dichotomous (pass/fail), we need a model that restricts predicted values to fall between 0 and 1, such as logistic regression.