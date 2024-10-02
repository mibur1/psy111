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

# X2.1 CFA in Python 

To compute an CFA in Python we will again use the `factor_analyzer` package. 

### Example dataset

For todays example, a dataset form the `` package is used. This dataset 

```{code-cell}
import statsmodels.api as sm
data = sm.datasets.get_rdataset('Harman74.cor', 'datasets')
print(data.data)
```