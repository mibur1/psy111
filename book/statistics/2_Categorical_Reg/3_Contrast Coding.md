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

# 6.3 Contrast Coding
For contrast coding, several approaches can be employed to address your research question. Due to its flexibility, we can define custom contrasts of interest. In this session, we will specify five different contrasts and test them on the data.

## Contrast Matrix

```{code-cell}
# Import necessary libraries
import numpy as np
import pandas as pd
import statsmodels.api as sm

data = pd.read_csv("data/alzheimers_data.txt", delimiter='\t')

# Define the contrasts of interest (here we try 5 different contrasts)
contrast_of_interest1 = np.array([-0.5, -0.5, 0, 0, 0.5, 0.5])
contrast_of_interest2 = np.array([0, 0, -0.5, -0.5, 0.5, 0.5])
contrast_of_interest3 = np.array([-0.5, 0, 0, 0.5, 0, 0])
contrast_of_interest4 = np.array([-0.5, -0.5, -0.5, 0.5, 0.5, 0.5])
contrast_of_interest5 = np.array([0.5, 0, 0, 0, 0, -0.5])

# Create the contrast matrix
contrast_matrix = np.column_stack((
    contrast_of_interest1,
    contrast_of_interest2,
    contrast_of_interest3,
    contrast_of_interest4,
    contrast_of_interest5
))

# Specify the genotypes in the correct order. The order is important for the index of the matrix.
genotypes = ['e2/e2', 'e2/e3', 'e2/e4', 'e3/e3', 'e3/e4', 'e4/e4']

# Create a DataFrame for the contrast matrix with genotypes as index
contrast_df = pd.DataFrame(contrast_matrix, index=genotypes, columns=[
    'contrast_of_interest1',
    'contrast_of_interest2',
    'contrast_of_interest3',
    'contrast_of_interest4',
    'contrast_of_interest5'
])

print(contrast_df)
```
We will obtain five different matrices, with the order of the genotypes determining the index of each matrix. Can you identify which genotype is being compared to which?
 `contrast_of_interest1` is comparing the combined effect of `e2/e2` and `e2/e3` (group 1) versus `e3/e4` and `e4/e4` (group 2). The genotypes `e2/e4` and `e3/e3` are not part of this contrast (they do not influence the comparison).

## Linaer Regression

```{code-cell}
# Ensure 'genotype' column is correctly handled and matches the contrast matrix genotypes
data['genotype'] = pd.Categorical(data['genotype'], categories=genotypes)

# Drop NaN values in relevant columns (both 'genotype' and 'WMf')
data = data.dropna(subset=['genotype', 'WMf'])

# Map each genotype in the dataset to the corresponding row in the contrast matrix
genotype_mapping = {"e2/e2": 0, "e2/e3": 1, "e2/e4": 2, "e3/e3": 3, "e3/e4": 4, "e4/e4": 5}

# Create the design matrix X using the contrast matrix for each genotype in the data
X = np.array([contrast_matrix[genotype_mapping[gen]] for gen in data['genotype']])

# Add a constant (intercept) to the design matrix
X = sm.add_constant(X)

# Define the dependent variable (y)
y = data['WMf']

# Fit the OLS model using the contrast matrix as the design matrix
model = sm.OLS(y, X).fit()

# Step 12: Print the summary of the model results
print(model.summary())
```
## What does the output tell me?

For `contrast_of_interest1`
Coefficient= 0.06: This indicates that the effect of contrast_of_interest1 (i.e., the comparison between `e2/e2` and `e2/e3` combined vs. `e3/e4` and `e4/e4` combined) on the dependent variable WMf is estimated to be 0.06058. This means that on average, individuals with the first combination of genotypes have a higher `WMf` score by approximately 0.06058 compared to those in the second combination.

```{admonition} Summary
:class: tip
- Contrast coding is the most flexible approach
- the entire matrix must be defined manually
```