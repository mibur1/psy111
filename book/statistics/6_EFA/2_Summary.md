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

Summed up, the usage of the `factor_analyzer` package is similar to previously introduced workflows for statistical modeling. Please read through the [documentation](https://factor-analyzer.readthedocs.io/en/latest/index.html) for a detailed overview.




https://factor-analyzer.readthedocs.io/en/latest/factor_analyzer.html


```{code-block}
fa_object = FactorAnalyzer(n_factors=3,
                           rotation='promax',
                           method='minres',
                           use_smc=True,
                           is_corr_matrix=False,
                           bounds=(0.005, 1),
                           impute='median',
                           svd_method='randomized',
                           rotation_kwargs=None)
```

For the `FactorAnalyzer` object, we have several options as described in the [documentation](https://factor-analyzer.readthedocs.io/en/latest/factor_analyzer.html). The most important ones are:

- `n_factors`: The number of factors
- `rotation`: The type of rotation to perform after fitting the factor analysis model
- `method`: The fitting method to use
- `is_corr_matrix` can be set to `Tue` if the data is already a correlation matrix

We can then fit the model and extract its estimates such as eigenvalues, loadings, and communalities:

```{code-block}
fa_object.fit(data)

ev, cfev = fa.get_eigenvalues()
l = fa2.loadings_
c = fa2.get_communalities()
```