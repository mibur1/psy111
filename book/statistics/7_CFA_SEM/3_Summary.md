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

## Specifying a Model

```{code-block}
desc = '''# Measurement model
          latent_factor1 =~ x1 + x2 + x3
          latent_factor2 =~ x7 + x8 + x9
          latent_factor3 =~ x4 + x5 + x6
          
          # Addding higher order factors
          latent_factor1 =~ latent_factor2
          latent_factor1 =~ latent_factor3

          # Structural model
          latent_factor1 ~ latent_factor2

          # Adding a covariance
          latent_factor2 ~~ latent_factor3
          
          # Setting a covariance to zero
          latent_factor1 ~~ 0*latent_factor3

          # Setting a factor variance to 1
          latent_factor1 ~~ 1 * latent_factor1'''
```

Summed up, you can use the following operators:

- `=~` to associate measured variables with latent factors (or latent factors with higher order latent factors)
- `~` for regressions
- `~~` for variances and covariances

## Fitting a Model

```{code-block}
mod = semopy.Model(desc)
res_opt = mod.fit(data)
```

## Extracting Model Estimates

```{code-block}
estimates = mod.inspect()
print(estimates)
```

## Extracting Model Fit Measures

```{code-block}
stats = semopy.calc_stats(mod)
print(stats.T)
```

## Visualizing the Model

```{code-block}
semopy.semplot(mod, plot_covs = True, filename='data/cfa_plot.pdf')
```
