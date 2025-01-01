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

# 10.2 Summary

Summed up, the usage of the `factor_analyzer` package is similar to previously introduced workflows for statistical modeling:


```{code-block}
fa_object = FactorAnalyzer(n_factors=3, rotation='promax', method='minres',
                           use_smc=True, is_corr_matrix=False,
                           bounds=(0.005, 1), impute='median',
                           svd_method='randomized', rotation_kwargs=None)
```

For the `FactorAnalyzer` object, we have several options. The most important ones are

- `n_factors` determines the number of factors. For determining the number of needed factors, begin by fitting a model where the number of factros is equal to the number of observed variables (see above).

- `rotation`determines the rotation algorithm used. Set `rotation = 'none'` for no rotation. See [factor_analyzer documentation](https://factor-analyzer.readthedocs.io/en/latest/index.html) for possible rotation options.

- `method` determines the the method to fit the model. Set `method = 'ml'` for Maximum Likelihood and `method = 'minres'` for MINRES.

- `is_corr_matrix` has to be set to `true` if the data is a correlation matrix. Defaults to `false`.

We can then continue with fitting the model:

```{code-block}
fa_object.fit(data)
```

and extracting model estimates such as eigenvalues, loadings, and communalities:

```{code-block}
ev, cfev = fa.get_eigenvalues()
l = fa2.loadings_
c = fa2.get_communalities()
```