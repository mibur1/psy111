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


# X.2 `factor_analyzer` sum'ed up

## Create a factor analysis object

```{code-cell, eval = FALSE}
fa_object = FactorAnalyzer(n_factors=3, rotation='promax', method='minres', use_smc=True, is_corr_matrix=False, bounds=(0.005, 1), impute='median', svd_method='randomized', rotation_kwargs=None)
```

- `n_factors` determines the number of factors. For determining the number of needed factors, begin by fitting a model where the number of factros is equal to the number of observed variables (see above). 

- `rotation`determines the rotation algorithm used. Set `rotation = 'none'` for no rotation. See [factor_analyzer documentation](https://factor-analyzer.readthedocs.io/en/latest/index.html) for possible rotation options. 

- `method` determines the the method to fit the model. Set `method = 'ml'` for Maximum Likelihood and `method = 'minres'` for MINRES. 

- `is_corr_matrix` has to be set to `true` if the data is a correlation matrix. Defaults to `false`.

- All other inputs can be ignored and kept as defaults. 

## Fit a model

```{code-cell, eval = FALSE}
fa_object.fit(data)
```

## Extract model values

```{code-cell, eval = FALSE}
# Eigenvalues 
ev, cfev = fa.get_eigenvalues() 
# cfev gives us the common factor eigenvalues , which we don't need at the moment. 
print(ev)

# Loadings 
l = fa2.loadings_
print(l)

# Communalities 
c = fa2.get_communalities()
print(c)
```

## Other packages

Please note that there are also other packages which can be used to apply EFA in Python. However, the `factor_analyzer` package stands out as the most comprehensive and reliable Python package for conducting EFA {cite}`Persson.2021`. Also, its EFA results align with those from the `psych` package in R. 