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


# 8.3 `statsmodels` sum'ed up

## Create features

```{code-cell}
polynomial_features = PolynomialFeatures(degree=order, include_bias=True) 
x_p = polynomial_features.fit_transform(x.reshape(-1, 1)) 
```
- `degree` specifies the order of the polynomial.
- `include_bias = True` adds an intercept.

## Fit a model and get residuals

```{code-cell}
model = sm.OLS(y, x_p).fit()
fit = model.predict(x_p)
residuals = model.resid
```

## Center a predictor

```{code-cell}
data['x_c'] = data['x'] - data['x'].mean()
```
