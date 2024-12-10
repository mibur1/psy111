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

# 6.2 Effects Coding

## Unweighted Effects Coding

For dummy coding, we used the `e4/e4` genotype as the reference category. The intercept represented the mean of the reference category, and the coefficients for other groups represented the difference in means between each group and the reference.

The difference in unweighted effects coding now is that the reference is the overall mean of all groups rather than a specific group. The intercept then represents the grand mean (overall mean of the dependent variable, WMf), while the coefficients for each group represent the deviation of that group's mean from the overall mean.

We can implement unweighted effects coding similarly to dummy coding but we will use `Sum` instead of `Treatment` for the contrast.

```{code-cell}
import numpy as np
import pandas as pd
from patsy.contrasts import Sum
import statsmodels.formula.api as smf

# Load the dataset
df = pd.read_csv("data/alzheimers_data.txt", delimiter='\t').dropna()

# Convert genotype into a categorical variable
df['genotype'] = df['genotype'].astype('category')

# Create and fit the model
model = smf.ols('WMf ~ C(genotype, Sum)', data=df)
results = model.fit()

print(results.summary())
```

You can see, that the result are fairly similar to dummy coding, as the grand mean is close to the `e4/e4` mean.

When it comes to the contrast matrix, it looks pretty similar, with the only distinction being last row coded as **-1**. In effect coding, there is is no explicit reference group as seen in dummy coding - the grand mean serves as the reference. The group coded with -1 is central to this coding scheme but doesn't act as a conventional reference category for comparisons.

```{code-cell}
# Get all genotype levels and save them as a list
levels = df['genotype'].cat.categories.tolist()

# Create the contrast matrix
contrast = Sum().code_without_intercept(levels)

print("Levels:", levels)
print("Contrast Matrix:\n", contrast.matrix)
```

In the matrix, each row corresponds to a level of the categorical variable. The coding scheme uses -1 for all variables in the last row to enforce the constraint that the sum of the coefficients is zero. This is the key difference from dummy coding. The columns of the matrix correspond to the levels of the categorical variable, excluding the last one (because the last level is redundant due to the zero-sum constraint).


## Weighted Effects Coding

While unweighted effects coding uses the grand mean (unweighted average of all groups) as the reference, weighted effects coding modifies this approach to use the weighted mean of the dependent variable as the reference. The weighted mean accounts for the group sizes, giving more weight to groups with larger sample sizes.

This approach is particularly useful when group sizes differ significantly, as it ensures the comparison is more representative of the overall data distribution.

The intercept in weighted effects coding represents the weighted mean of the dependent variable (`WMf`), while the coefficients for each group represent the deviation of that group’s mean from the weighted mean.

As this functionality is not directly offered, we will create the design matrix manually by performing the following steps:

1.  Computing the sample proportions for each category in the categorical variable

1. Calculate the sample sizes and proportion for each category:

```{code-cell}
# calculating the counts of unique genotype levels in the column 'genotype'
genotype_counts = df['genotype'].value_counts(sort=False)
# Extracting the numerical counts(frequency) associated with each genotype level
counts = genotype_counts.values

print("Genotype Levels:", levels)       
print("Counts:", counts)

```

3.  Use these counts to create custom weights for the reference category

```{code-cell}
contrast_matrix = {
    "e2/e2": np.array([1, 0, 0, 0, 0]),
    "e2/e3": np.array([0, 1, 0, 0, 0]),
    "e2/e4": np.array([0, 0, 1, 0, 0]),
    "e3/e3": np.array([0, 0, 0, 1, 0]),
    "e3/e4": np.array([0, 0, 0, 0, 1]),
    "e4/e4": -counts[:-1] / counts[-1]
}

# Print each genotype's corresponding contrast vector
for key, value in contrast_matrix.items():
    print(f"{key}: {value}\n")
```

4.  Create the weighted effects coding design matrix and outcome vector

```{code-cell}
import statsmodels.api as sm

# Build the design matrix (X)
X = np.array([contrast_matrix[genotype] for genotype in df['genotype']])

# Add intercept
X = sm.add_constant(X)  

print(X)
print("Design matrix shape:", X.shape)
print("Column sums:", np.round(np.sum(X, axis=0), 2))
print("e3/e3 column:\n", X[:,4])

# Define the target vector (outcome variable)
y = df['WMf']
```

We added some print statements to see what is going on inside the design matrix, and if everything is correct:
  - Shape: (245, 6)
    - 245 rows: Matches the number of observations in your dataset (df)
    - 6 columns: Matches the expected design matrix structure:
      - Intercept (constant column)
      - 5 contrast-coded columns for the categorical variable genotype
  - Column sums: [245. -0. -0. -0. 0. 0.]
    - The sum of the first column is 245, which corresponds to the number of observations (all intercept values are 1)
    - The sums of the other columns are 0, satisfying the sum-to-zero constraint of weighted effects coding
  - Inspecting a single column
    - The fifth column (`e3/e3`) contains a mix of 1, 0, and -14.3 values
      - 1 indicates the observation belongs to this genotype
      - 0 indicates the observation does not belong to this genotype
      - 14.3 indicates the observation belongs to last group (e4/e4) and codes its contribution to the weighted mean, accounting for the imbalance in group sizes to maintain the sum-to-zero constraint

5. Create and fit the model. Note that we now use `OLS()` from `statsmodels.api` instead of `ols()` from `statsmodels.formula.api`, as we do not provide a formula but define the regression model in a mathematical way through the design matrix:

```{code-cell}
model = sm.OLS(y, X)
results = model.fit()
print(results.summary())
```

**Interpretation of the output:**

1. Intercept:
    - In weighted effects coding, the intercept represents the weighted mean of WMf, not the grand mean.
    - The weighted mean is calculated by giving each group a weight proportional to its size:
        
    $$\text{Weighted Mean} = \frac{\sum (\text{Group Size} \times \text{Group Mean})}{\sum (\text{Group Size})}$$

2. Coefficients:
    - Each coefficient represents the deviation of the group mean from the weighted mean, taking group sizes into account.
    - Larger groups have more influence on the weighted mean, so coefficients for smaller groups may differ more significantly compared to unweighted effects coding.


```{admonition} Summary
:class: tip
- Unweighted effects coding compares all groups to the grand mean, which is the unweighted average of the dependent variable. The intercept represents the grand mean, serving as the baseline for interpretation.

- Weighted effects coding compares all groups to the weighted mean, which accounts for group sizes. The negative values in the design matrix reflect proportional adjustments needed to satisfy the sum-to-zero constraint.
```