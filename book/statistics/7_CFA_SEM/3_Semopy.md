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

# 11.3 Summary

## Specify a model

```{code-cell}
desc = '''
# Measurement model
latent_factor1 =~ x1 + x2 + x3
latent_factor2 =~ x7 + x8 + x9
latent_factor3 =~ x4 + x5 + x6
# Add a higher order factor, i.e. latent factors loading onto a higher order latent factor
latent_factor1 =~ latent_factor2
latent_factor1 =~ latent_factor3

# Structural model
latent_factor1 ~ latent_factor2

# Additional covariances
# Adding a covariance between latent_factor2 and latent_factor3
latent_factor2 ~~ latent_factor3
# Setting the covariance from to latent factors to zero
latent_factor1 ~~ 0*latent_factor3

# Setting a factor variance to 1
latent_factor1 ~~ 1 * latent_factor1 # Also an option to identify factors
'''
```

- Use the `=~` operator to associate measured variables with latent factors and associate latent factors with higher order latent factors.
- Use the `~` operator to assign regression
- Use the `~~` operator to assign covariances

## Fit a model

```{code-cell}
mod = semopy.Model(desc)
res_opt = mod.fit(data)
```

## Extract model estimates

```{code-cell}
estimates = mod.inspect()
print(estimates)
```

## Extract model fit measures

```{code-cell}
stats = semopy.calc_stats(mod)
print(stats.T)
```