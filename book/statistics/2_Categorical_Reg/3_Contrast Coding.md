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

Contrast coding allows us to define and test custom comparisons, making it more flexible than methods like dummy or effects coding. By specifying contrasts, we can address specific research questions, such as comparing specific groups. This approach is particularly useful when we want to investigate targeted hypotheses that are not straightforwardly addressed by other coding schemes.

We will explore contrast coding by defining five distinct contrasts and applying them to our dataset. Each contrast corresponds to a separate research question, showcasing how this approach provides tailored insights into group differences.


## Creating the Contrast Matrix

1. Load and prepare the data

```{code-cell}
import numpy as np
import pandas as pd
import statsmodels.api as sm

# Load the data, convert genotype to a categorical variable, and get the levels
df = pd.read_csv("data/alzheimers_data.txt", delimiter='\t').dropna()
df['genotype'] = df['genotype'].astype('category')
levels = df['genotype'].cat.categories.tolist()
print(levels)
```

2. Defining contrasts of interests and create the matrix

```{code-cell}
# levels:            'e2/e2', 'e2/e3', 'e2/e4', 'e3/e3', 'e3/e4', 'e4/e4'
contrast1 = np.array([-0.5,-0.5,0,0,0.5,0.5])
contrast2 = np.array([0,0,-0.5,-0.5,0.5,0.5])
contrast3 = np.array([-0.5,0,0,0.5,0,0])
contrast4 = np.array([-0.5,-0.5,-0.5,0.5,0.5,0.5])
contrast5 = np.array([0.5,0,0,0,0,-0.5])

contrast_matrix = np.column_stack([contrast1, contrast2, contrast3, contrast4, contrast5])
```

- Contrast 1: Compare the mean `WMf` of `e2/e2` and `e2/e3` against the mean of `e3/e4` and `e4/e4` 
- Contrast 2: Compare the mean `WMf` of `e2/e4` and `e3/e3` against the mean of `e3/e4` and `e4/e4`
- Contrast 3: Compare the mean `WMf` of `e2/e2` against the mean of `e3/e3`
- Contrast 4: Compare the mean `WMf` of `e2/e2`, `e2/e3`, and `e2/e4` against the mean of `e3/e3`, `e3/e4`, and `e4/e4`
- Contrast 5: Compare the mean `WMf` of `e2/e2` against the mean of `e4/e4`


## Creating the Regression Model

```{code-cell}
# Create mapping for the contrast matrix
genotype_mapping = {"e2/e2": 0, "e2/e3": 1, "e2/e4": 2, "e3/e3": 3, "e3/e4": 4, "e4/e4": 5}

# Create the design matrix and outcome variable
X = np.array([contrast_matrix[genotype_mapping[genotype]] for genotype in df['genotype']])
X = sm.add_constant(X)
y = df['WMf']

# Fit the model
model = sm.OLS(y, X)
results = model.fit()
print(results.summary())
```


## Interpreting the Outputs

- R-squared:
  - Only 5.2% of the variance in `WMf` is explained by the contrasts. This suggests that the model is fairly poor. The adjusted R-squared, which accounts for the number of predictors is even lower.

- F-statistic:
  - The overall model is statistically significant, indicating that the contrasts collectively contribute to explaining `WMf` (p=0.0257).

- Intercept (const):
  - Represents the grand mean of `WMf` across all genotypes.
  - Value: 0.8176 (highly significant, p<0.001).

- Contrast 1 (x1​):

  - Compares the mean `WMf` of `e2/e2` + `e2/e3` (Group 1) to `e3/e4` + `e4/e4` (Group 2).
  - Coefficient: 0.06060, indicating that Group 1 has a slightly higher mean `WMf` than Group 2.
  - p=0.402: This difference is not statistically significant.

- All other contrasts are interpreted similarly.

- Discussion:
  - The low R-squared shows the model does not capture much variance in `WMf`.
  - Non-significant results for the contrasts imply that the hypothesized contrasts do not strongly impact `WMf`.


```{admonition} Summary
:class: tip
Contrast coding is a flexible tool for testing targeted hypotheses.
```