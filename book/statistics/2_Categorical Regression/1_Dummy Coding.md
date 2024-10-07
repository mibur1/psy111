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
# 6.1 Dummy Coding
Dummy coding enables the comparison of each category of a categorical variable to a reference category, which serves as the baseline for measuring the effects of the other categories. In this coding scheme, the reference category is assigned a value of 0 (corresponding to the intercept), while the remaining dummy variables for the other categories are assigned values of either 1 or 0. As always, there are numerous packages available for implementing dummy coding. For this session, we will focus on `patsy()` from `Statsmodels`, as it provides the most straightforward and customizable options, particularly for defining the reference category.

First, we will import the essential libraries needed to visualize the data.

```{code-cell}
import pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt
import statsmodels.formula.api as smf

```
Load the data from your files and have a look at it.
```{code-cell}
df = pd.read_csv(r'C:\Users\laptop\Documents\HiWi\R_alte_Materialien\psy111\Seminar\2_2_categorical_regression\data.txt', delimiter='\t')  
print(df)
```
Ensure that `genotype` is treated as a categorical variable.

```{code-cell}
df['genotype'] = df['genotype'].astype('category')
```

Plot the `genotypes` with `seaborn` as individual boxplots against `WMf`

```{code-cell}
sns.boxplot(x='genotype', y='WMf', data=df)
plt.title("Boxplot pf genotype against WMf")
plt.show()
```
Now we proceed to calculate the dummy code matrix with `patsy`. 

```{code-cell}
import pandas as pd
from patsy import dmatrix
from patsy.contrasts import Treatment
import statsmodels.formula.api as smf

# Load the dataset
df = pd.read_csv(r'C:\Users\laptop\Documents\HiWi\R_alte_Materialien\psy111\Seminar\2_2_categorical_regression\data.txt', delimiter='\t')

# Ensure 'genotype' is treated as categorical
df['genotype'] = df['genotype'].astype('category')

# Define the levels of 'genotype' and set the reference
# Here 'e4/e4' is the reference category, so levels should exclude it.
levels = df['genotype'].cat.categories.tolist()  # Get the actual categories


# Create a Treatment contrast matrix with the specified reference category
contrast = Treatment(reference="e4/e4").code_without_intercept(levels)
print(contrast.matrix)

```
The final step of computing the dummy code matrix can be skipped if you are not interested in the matrix itself. You can simply execute the following line of code, as `Statsmodels` directly manages categorical encoding. There is no need to manually create dummy variables or use `dmatrix` unless you require custom contrasts beyond what the `Treatment` option offers.

```{code-cell}
#Use ols() for linear regression from the previous seminar
model = smf.ols('WMf ~ C(genotype, Treatment(reference="e4/e4"))', data=df).fit()

# Print the summary
print(model.summary())
```
The output explained:

- Coef: Contains the coefficients for each `genotype` of the dummy regression (Intercept and slope)
The following regression equation was fitted

$$\hat{Y} = 0.82 - 0.03 \cdot C_1 - 0.02 \cdot C_2 + 0.06 \cdot C_3 + 0.02 \cdot C_4 - 0.03 \cdot C_5$$

- intercept: indicates the mean of WMf (0,8185) in the reference category (e4/e4)

- slope of C1: indicates the WMf difference (-0.0333) between the reference category e4/e4 and the group C1 (e2/e2) coded by the dummy variable C1

- all other slopes are analogous to C1.
```{admonition} Summary
:class: tip
- in dummy coding, the reference category is assigned a value of 0.
- use `lm_model <- lm(WMf ~ C(genotype, Treatment(reference="your reference")), data=dat)` for dummy coding

```