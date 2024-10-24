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

# Polynomial Regression

Polynomial regression is a non-linear modeling approach that extends the linear model by adding nth-degree polynomial predictors. The advantage of using polynomial regression is that we can relax the linearity assumption with respect to the relationships between a predictor and an outcome, while still attempting to maintain as much interpretability as possible.

**Think of the following example.**

We want to predict your exam grade (from 0% to 100%) by the amount of hours you learned. One might think that there's a linear relationship between these variables, i.e. the more hours leanred the higher the grade. However, one might also argue that at one point one can learn too much, leading to e.g. reduced sleep, which negatively affects the exam grade. Thus, a linear relationship might underestimate the complexity of the association. A second degree polynomial might be better at representing the relationship, predicting increasing exam grades with increasing hours learned only until some point (e.g. 9 hours per day). From that 'breaking point' the model predicts decreasing grades with increasing hours learned. See the plot below for a visualization.

```{code-cell}
# You can ignore this chunk
import numpy as np
import matplotlib.pyplot as plt

hours = np.linspace(0, 24, 500)  
h = 9  
k = 100 
a = 100 / (9**2) 
grades = -a * (hours - h)**2 + k
grades = np.clip(grades, 0, 100)  

plt.figure(figsize=(8, 5))
plt.plot(hours, grades, label="Exam Grade vs. Hours Studied", color="blue", lw=2)
plt.xlabel("Hours Learned per Day", fontsize=12)
plt.ylabel("Exam Grade (%)", fontsize=12)
plt.title("Exam Grade as a Function of Hours Learned per Day", fontsize=14)
plt.xlim(0, 17)
plt.ylim(0, 100)
plt.grid(True)
plt.show()
```

## Curvilinear relationships

Lets briefly recap the concept of polynomials and how to detect them graphically.

$\hat{y} = b_0 + b_1 \cdot x + b_2 \cdot x^2 + ... + b_j \cdot x^{n-1}$

The polynomial equation relates one variable X (with n observations) to Y by using n-1 aspects of X: Given that only one observation was made for each potential value of X, an equation for a function that fits the data perfectly can be achieved by including n-1 polynomials.

To detect non-linear relationships between X and Y, in the lecture you learned about residuals plot. In the following, we create such a plot based on the same example as in the lecture.