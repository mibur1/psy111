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

This section summarises the key steps and syntax for specifying, fitting, inspecting, and visualising CFA and SEM models using `semopy`.

---

## Specifying a Model

In SEM, models are defined using a string-based syntax that separates the **measurement model** (relationships between observed and latent variables) from the **structural model** (relationships between latent variables).

```{code-block}
desc = '''# Measurement model
          latent_factor1 =~ x1 + x2 + x3
          latent_factor2 =~ x4 + x5 + x6

          # Structural model
          latent_factor2 ~ latent_factor1'''
```

This example specifies two latent variables, each measured by three observed variables, and a structural regression in which `latent_factor1` predicts `latent_factor2`.

---

### Higher-order Factors

Latent variables can themselves be indicators of a higher-order latent variable:

```{code-block}
desc = '''# First-order measurement model
          latent_factor1 =~ x1 + x2 + x3
          latent_factor2 =~ x4 + x5 + x6

          # Higher-order factor
          general_factor =~ latent_factor1 + latent_factor2'''
```

---

### Variances and Covariances

Variances and covariances are specified using the `~~` operator:

```{code-block}
desc = '''latent_factor1 =~ x1 + x2 + x3
          latent_factor2 =~ x4 + x5 + x6

          # Allow latent factors to covary
          latent_factor1 ~~ latent_factor2
          '''
```

Covariances can be fixed to zero to impose independence assumptions:
```{code-block}
latent_factor1 ~~ 0*latent_factor2
```
---

### Overview of Operators

* `=~` associates observed variables with latent factors (and latent factors with higher-order factors)
* `~` specifies regression relationships
* `~~` specifies variances and covariances

---

## Fitting a Model

```{code-block}
model = semopy.Model(desc)
model_fit = model.fit(data)
```

---

## Extracting Model Estimates

```{code-block}
estimates = model.inspect(std_est=True)
print(estimates)
```

---

## Extracting Fit Measures

```{code-block}
stats = semopy.calc_stats(model)
print(stats.T)
```

---

## Visualising the Model

```{code-block}
semopy.semplot(model, plot_covs=True, std_ests=True, filename='data/plot.pdf')
```

---

```{admonition} Key Takeaways
:class: note

- CFA focuses on the measurement model; SEM extends CFA by adding directional relationships between latent variables.
- SEM models consist of a measurement model and a structural model.
- Individual parameter estimates and overall model fit address different questions and must be interpreted jointly.
- Standardised estimates aid interpretation, while unstandardised estimates are required for statistical inference.
```