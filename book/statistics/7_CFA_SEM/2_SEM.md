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

# 11.2 SEM

As in the CFA example, we again use the `HolzingerSwineford1939` dataset, which contains mental ability test scores from seventh- and eighth-grade pupils in two schools.

```{code-cell} ipython3
import semopy

data = semopy.examples.holzinger39.get_data()
data
```

In Confirmatory Factor Analysis (CFA), we specify how observed variables measure latent constructs and allow latent variables to correlate. CFA focuses on the measurement model and does not include directional (regression) relationships between latent variables.

Structural Equation Modelling (SEM) extends CFA by allowing directional relationships among latent variables. An SEM therefore consists of two components:

- a **measurement model**, which specifies how observed variables relate to latent variables, and
- a **structural model**, which specifies regressions among latent variables.

Whenever at least one latent variable is used to predict another latent variable, the model is considered an SEM. As an example, we now specify a model in which `visual ability` predicts `speed ability`. Text-related abilities are not included in this model.

```{code-cell} ipython3
# Specify the model
desc = '''# Measurement model
          visual =~ x1 + x2 + x3
          speed =~ x7 + x8 + x9

          # Structural model
          speed ~ visual'''

# Fit the model
model = semopy.Model(desc)
results = model.fit(data)

# Visualize the model
semopy.semplot(model, plot_covs = True, std_ests=True, filename='data/sem_plot.pdf')
```

## Model Estimates

```{code-cell} ipython3
estimates = model.inspect(std_est=True)
print(estimates)
```

For guidance on interpreting factor loadings, (co)variances, and residual variances, please refer to the previous chapter. Here, we focus on the newly introduced structural regression: `speed ~ visual`

* The `Estimate` column contains the unstandardised regression coefficient, representing the expected change in *speed ability* for a one-unit increase in *visual ability* (on the latent scale).
* The `Est. Std` column contains the standardised regression coefficient, representing the expected change (in standard deviations) in *speed ability* for a one-standard-deviation increase in *visual ability*.

The regression coefficient is significantly different from zero (see `p-value`), indicating that visual ability significantly predicts speed ability within this model.

---

## Model Fit

```{code-cell} ipython3
stats = semopy.calc_stats(model)
print(stats.T)
```

To assess how well the model reproduces the observed data, we examine the model fit indices (see the previous chapter for details).

* The **χ² test** is significant, indicating that the model-implied covariance matrix differs from the observed covariance matrix.
* **CFI (≈ 0.88)** and **TLI (≈ 0.77)** fall below commonly used thresholds for acceptable fit.
* **RMSEA (≈ 0.13)** exceeds typical cut-offs for adequate model fit.

Taken together, these indices suggest that the model provides a poor overall fit to the data. Although the regression from visual ability to speed ability is statistically significant, the model does not adequately capture the covariance structure of the observed variables.

---

```{admonition} Summary
:class: note

Structural Equation Modelling allows us to test **directional hypotheses between latent variables**, but a statistically significant regression path does not guarantee good overall model fit. Both parameter estimates and global fit measures must be considered when evaluating an SEM.
```
