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


# 10.2 `statsmodels` sum'ed up

## Create a and fit model

```{code-cell}
model = smf.mixedlm("Y ~ X", data, groups=data["Group"], re_formula="~X")
model_fit = model3.fit(method="bfgs")
```
- `Y` specifies the dependent variable.
- `X` specifies the independent variable (predictor).
- `data` specifies the dataset
- `groups = data["Group"]` specifies the grouping variable (level 2 variable).
- `re_formula="~X"` specifies the random slopes. In this case, random slopes for the predictor `X` will be fitted. Not specifiyng this term leads to the model not having random slopes (but only random intecepts).

## Print model summary 

```{code-cell}
print(model_fit.summary())
```



