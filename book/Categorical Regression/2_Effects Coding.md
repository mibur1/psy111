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
# 6.2 Unweighted Effects Coding
As for dummy coding we assign e4/e4 as the reference category. The only difference for unweighted effects coding is, that the reference category is coded as -1 for all coding variables. We will also use `patsy()` from `Statsmodels`.

```{admonition} 
:class: warning
As you might want to look up Unweighted effects coding, you will encounter different names for it such as Sum(Deviation) Coding.
```
We proceed almost like dummy coding but use `sum()` for the contrast.

```{code-cell}
import pandas as pd
from patsy import dmatrix
from patsy.contrasts import Sum

# Load the dataset
df = pd.read_csv(r'C:\Users\laptop\Documents\HiWi\R_alte_Materialien\psy111\Seminar\2_2_categorical_regression\data.txt', delimiter='\t')

# Ensure 'genotype' is treated as categorical
df['genotype'] = df['genotype'].astype('category')

# Define the levels of 'genotype'
levels = df['genotype'].cat.categories.tolist()

# Create a Sum deviation contrast matrix
from patsy.contrasts import Sum

contrast = Sum().code_without_intercept(levels)

print(contrast.matrix)

model = smf.ols('WMf ~ C(genotype, Treatment(reference="e4/e4"))', data=df).fit()

# Print the summary
print(model.summary())
```
The model summary shows similiar results as for dummy coding.

# Weighted Effects Coding

In weighted effect coding, the reference group is assigned a sample weight for all coding variables instead of -1. 

Thherefore we execute the following steps manually:
1. Compute the sample proportions for each category in your categorical variable.
2. Use those proportions to create custom weights for the reference category.
3. Implement this weighted effect coding manually.

