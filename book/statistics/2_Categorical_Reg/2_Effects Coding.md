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
For dummy coding, we designate the e4/e4 genotype as the reference category. The only distinction with unweighted effects coding is that the reference category is coded as -1 across all coding variables. We will also utilize `patsy()` from `Statsmodels` for this process.
```{admonition}
:class: warning
As you might want to look up Unweighted effects coding, you will encounter different names for it such as Sum(Deviation) Coding.
```
We proceed almost like dummy coding but use `sum()` for the contrast.

```{code-cell}
import numpy as np
import pandas as pd
from patsy import dmatrix
from patsy.contrasts import Sum
import statsmodels.formula.api as smf

# Load the dataset
df = pd.read_csv("data/data.txt", delimiter='\t')

# Ensure 'genotype' is treated as categorical
df['genotype'] = df['genotype'].astype('category')

# Define the levels of 'genotype'
levels = df['genotype'].cat.categories.tolist()

# Create a Sum deviation contrast matrix
from patsy.contrasts import Sum

contrast = Sum().code_without_intercept(levels)

print(contrast.matrix)
```
```{code-cell}
model = smf.ols('WMf ~ C(genotype, Treatment(reference="e4/e4"))', data=df).fit()

# Print the summary
print(model.summary())
```
The model summary shows similiar results as for dummy coding.

# Weighted Effects Coding

In weighted effects coding, the reference group is assigned a sample weight for all coding variables instead of -1.

Therefore, we will execute the following steps manually:

1.  Compute the sample proportions for each category in the categorical variable.
2.  Use these proportions to create custom weights for the reference category.
3.  Implement the weighted effects coding manually.

```{code-cell}
data = df # TODO: refactor

#Check for NaN values in the 'genotype' column
print("NaN values in genotype column:", data['genotype'].isna().sum())

# Drop rows with NaN values
data = data.dropna(subset=['genotype'])

# Calculate sample sizes for each genotype
genotype_counts = data['genotype'].value_counts()
Nt1 = genotype_counts.get("e2/e2", 0)
Nt2 = genotype_counts.get("e2/e3", 0)
Nt3 = genotype_counts.get("e2/e4", 0)
Nt4 = genotype_counts.get("e3/e3", 0)
Nt5 = genotype_counts.get("e3/e4", 0)
Nt6 = genotype_counts.get("e4/e4", 0)  # Reference category

# Manually calculate weights for weighted effect coding
e2e2_we = np.array([1, 0, 0, 0, 0])
e2e3_we = np.array([0, 1, 0, 0, 0])
e2e4_we = np.array([0, 0, 1, 0, 0])
e3e3_we = np.array([0, 0, 0, 1, 0])
e3e4_we = np.array([0, 0, 0, 0, 1])

# ...and for the refernece category ##ATTENTION:maybe you want to delete the loop(?)
if Nt6 > 0:
    e4e4_weights = np.array([-Nt1/Nt6, -Nt2/Nt6, -Nt3/Nt6, -Nt4/Nt6, -Nt5/Nt6])
else:
    raise ValueError("Sample size for reference category 'e4/e4' cannot be zero.")

# Define the full contrast matrix (weighted effect coding)
contrast_matrix = {
    'e2/e2': e2e2_we,
    'e2/e3': e2e3_we,
    'e2/e4': e2e4_we,
    'e3/e3': e3e3_we,
    'e3/e4': e3e4_we,
    'e4/e4': e4e4_weights  # Reference category
}
print(contrast_matrix)
```

```{code-cell}
# Create the design matrix (X)
X = np.array([contrast_matrix[genotype] for genotype in data['genotype']])

# Add a constant (intercept) to the design matrix
X = sm.add_constant(X)

#Define the dependent variable  WMf (y)
y = data['WMf']

# Fit the OLS model using the contrast matrix
model = sm.OLS(y, X).fit()

# Print the summary of the results
print(model.summary())
```
```{admonition} Summary
:class: tip
- for unweighetd effects coding use: `contrast = Sum().code_without_intercept(levels)`
- weighted effects coding must be manually implemented
```