
# 11.2 Summary

Summed up, running an EFA with `statsmodels` follows the same fit-then-inspect workflow as the other models in this book. Please read through the [documentation](https://www.statsmodels.org/stable/generated/statsmodels.multivariate.factor.Factor.html) for a detailed overview.

```python
from statsmodels.multivariate.factor import Factor

fa = Factor(endog=data,      # a DataFrame of observed variables
            n_factor=3,      # the number of factors to extract
            method="ml",     # "ml" (maximum likelihood) or "pa" (principal axis)
            corr=None,       # pass a correlation matrix here instead of raw data
            smc=True).fit()  # squared multiple correlations as initial communalities

fa.rotate("oblimin")         # rotate in place
```

The most important options are:

- `n_factor`: the number of factors
- `method`: the fitting method, `"ml"` for maximum likelihood or `"pa"` for principal axis
- `corr`: set this if you already have a correlation matrix rather than raw data
- `rotate(method)`: the rotation applied after fitting. Orthogonal options include `varimax` and `quartimax`; oblique options include `oblimin`, `quartimin` and `promax`

We can then extract the estimates such as eigenvalues, loadings, and communalities:

```python
import numpy as np

eigenvalues   = np.sort(np.linalg.eigvalsh(data.corr()))[::-1]  # for the Kaiser criterion
loadings      = np.real_if_close(fa.loadings)                   # item x factor matrix
communalities = 1 - fa.uniqueness                               # variance explained per item

print(fa.summary())   # a formatted overview of loadings and uniquenesses
```

```{note} Rotation caveat
The `statsmodels` documentation notes that `varimax`, `quartimax` and `oblimin` are verified against R and Stata, while some other methods (including `promax`) may not reproduce the defaults of those packages exactly. If you need to match a published R result, check which rotation and which normalisation it used.
```
