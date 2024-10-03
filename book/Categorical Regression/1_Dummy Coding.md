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
Dummy coding allows you to compare each category of a categorical variable to a reference category, with the reference category’s effect serving as the baseline against which the effects of other categories are measured. The reference category is assigned a value of 0 (which corresponds to the intercept), while the other dummy variables represent the remaining categories and are assigned values of either 1 or 0.
As always there are lots of different packages to choose from for dummy coding. We will focus on `patsy()` from `Statsmodels` as it offers the easiest and most customizable options, especially for the reference category.

First we import all the basic librarys to visualize the data

```{code-cell}
import pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt
import statsmodels.formula.api as smf

```
Load the data from your files.
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
#import the librarys
from patsy import dmatrix
from patsy.contrasts import Treatment

# Get the actual categories
levels = df['genotype'].cat.categories.tolist() 

#define the reference category as e4/e4
reference = 'e4/e4'

#Create a Treatment contrast matrix with the specified reference category
ccontrast = Sum().code_without_intercept(levels)

print(contrast.matrix)

```
The last step of computing the dummy code matrix can be skipped, if you are not interested in matrix. You can also just execute the following lien of code as `Statsmodel` directly handles categorical encoding. You don’t need to manually create dummy variables or use `dmatrix` unless you need custom contrasts beyond what `Treatment` provides.

```{code-cell}
#Use ols() for linear regression from the previous seminar
model = smf.ols('WMf ~ C(genotype, Treatment(reference="e4/e4"))', data=df).fit()

# Print the summary
print(model.summary())
```
The output explained:

- Coef: COntains the coefficients for each `genotype` of the dummy regression (Intercept and slope)
The following regression equation was fitted

$$\hat{Y} = 0.82 - 0.03 \cdot C_1 - 0.02 \cdot C_2 + 0.06 \cdot C_3 + 0.02 \cdot C_4 - 0.03 \cdot C_5$$

- intercept: indicates the mean of WMf (0,8185) in the reference category (e4/e4)

- slope of C1: indicates the WMf difference (-0.0333) between the reference category e4/e4 and the group C1 (e2/e2) coded by the dummy variable C1

- all other slopes ina anlogie to C1
