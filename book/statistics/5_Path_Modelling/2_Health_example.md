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

# 5.3 Health example
Now, let’s apply path modeling to a real-world example to better understand the meaning of the output. In this example, we will investigate the relationships between Physical Health ($X$), Functional Health ($Y_1$), and Subjective Health ($Y_2$), as proposed by Whitelaw and Liang (1991).

We’ve formulated the following hypotheses for our investigation:

1. Physical health influences functional health, which, in turn, predicts subjective health.
2. Physical health directly affects subjective health.

Based on these hypotheses, we structure the relationships into the following diagram.

**Variables**
- Physical Health ($X$): The number of illnesses experienced in the last 12 months.
- Functional Health ($Y_1$): The sum score of the SF-36 questionnaire, which measures functional health.
Subjective Health ($Y_2$): Self-reported subjective health, reflecting how individuals perceive their overall health.

We use the following code:
```{code-cell}
#libraries
import pandas as pd
import semopy
import matplotlib.pyplot as plt
import graphviz
from semopy import calc_stats

#import the dataset
df = pd.read_csv(r'C:\Users\laptop\Documents\HiWi\R_alte_Materialien\psy111\Seminar\2_5_path_modelling\2Health.txt', delim_whitespace=True)
#ATTENTION: maybe you would like to chnage the delim_whitespace=True to something else as this might get deleted as valid syntax soon
print(df.head())

#name the columns
df.columns = ["SubjHealth","SubjHealthChange", "PhysicHealth", "NrDoctorApp", "FunctHealth", "FunctHealth1", "FunctHealth2"]
print(df.head())

#define our model from our hypotheses like the following
desc = """
# Regression analysis
SubjHealth ~ PhysicHealth + FunctHealth
FunctHealth ~ PhysicHealth
"""

# Initialize and fit the model
mod = semopy.Model(desc)
res = mod.fit(df)

# Print the fitting results
print(res)

# Inspect the results (parameter estimates, fit indices, etc.)
estimates = mod.inspect(std_est= True)
print(estimates)

#print out the model fit statistic
stats = calc_stats(mod)
print(stats)

#Optional: save the relaionship plot
#model_diagram=semopy.semplot(mod, "health.png") 
```
## Model Output
Objective and Optimization

- *Objective Function*: Maximum Likelihood with Weights (MLW).
- *Optimization Algorithm*: Sequential Least Squares Quadratic Programming (SLSQP), chosen based on model requirements and complexity.
- *Number of Iterations*: The model converged after 14 iterations, indicating a quick and efficient optimization process.

**Parameter Estimates**
The following are the key relationships identified in the model:

`Functional Health ~ Physical Health`
- Coefficient: -0.096
- Standard Error: 0.0047
- z-value: -20.527
- p-value: < 0.001
Interpretation: Physical health has a significant negative effect on functional health. As the number of illnesses increases, functional health decreases.

`Subjective Health ~ Physical Health`
- Coefficient: -0.094
- Standard Error: 0.008
- z-value: -11.39
- p-value: < 0.001
Interpretation: Physical health (the number of illnesses) has a significant negative effect on subjective health. More illnesses correlate with poorer subjective health.

`Subjective Health ~ Functional Health`
- Coefficient: 0.876
- Standard Error: 0.039
- z-value: 22.41
- p-value: < 0.001
Interpretation: Functional health has a significant positive effect on subjective health. Higher functional health scores lead to better subjective health.

**Standardized Coefficients (Relative Importance)**

Standardized coefficients (`Est.Std`) allow us to compare the relative influence of each predictor:

- Physical Health → Functional Health: -0.451 (strong negative effect).
- Physical Health → Subjective Health: -0.244 (moderate negative effect).
- Functional Health → Subjective Health: 0.481 (substantial positive effect).

**Variance Explained**

- Functional Health: Variance = 0.172 (p < 0.001). This represents the unexplained variability in functional health.
- Subjective Health: Variance = 0.432 (p < 0.001). This represents the unexplained variability in subjective health.

## Model Evaluation
Several metrics help us evaluate the model fit, but we will focus on the following:

1. *Chi-Square Value*: A low value suggests a good fit.
In our model, the extremely low Chi-Square value and high p-value indicate that the model fits the data almost perfectly.
2. *Objective Value*: The objective value of 0.000 suggests no detectable discrepancy between the observed and model-predicted covariance matrices.

## Interpretation
The results provide meaningful insights into the relationships between the variables:

- Subjective health is strongly influenced by both physical and functional health.
- Functional health is significantly affected by physical health.
- Specifically, better subjective health is associated with fewer illnesses and higher functional health scores, while better functional health is tied to fewer illnesses in the past year.

```{admonition} Caution
:class: warning
While the model demonstrates significant and meaningful relationships, the almost perfect fit (e.g., Chi-Square value close to zero, p-value very high, and objective value of 0.000) raises a valid question: **Is the model overfitting?**

- Overfitting could mean that the model may not generalize well to other datasets.
- To address this, consider testing the model on a different dataset or simplifying it to ensure robustness.
```