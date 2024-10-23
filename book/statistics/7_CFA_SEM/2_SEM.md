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

# 7.2 SEM in Python

To compute an SEM in Python we will use the `semopy` package again. 

## Example dataset

Also similar to before, we will use the `HolzingerSwineford1939` dataset.

```{code-cell}
# Load the package
import semopy
# Load and inspect the dataset 
data = semopy.examples.holzinger39.get_data()
```

## Specify and fit the model

As you know, CFA is a special case of SEM which is defined by not having unidirectional paths present at one level, i.e. no latent variable is used to predict another latent variable (only correlations, i.e. bidirectional paths are used). But what if we suspect that one latent factor is actually predicting another one. Such models would be considered SEM.

Note that SEM models contain a **measurement model** and a **structural model**. The **measurement model** describes relationships between measured variables and latent factors. The **structural model** describes relationships between latent variables.

Let's specify and fit a SEM model that predicts `speed ability` with `visual speed` and ignores `text processing related abilities`.

```{code-cell}
# Specify the model
desc = '''
# Measurement model
visual =~ x1 + x2 + x3
speed =~ x7 + x8 + x9

# Structural model
speed ~ visual'''

# Fit the model
mod = semopy.Model(desc)
res_opt = mod.fit(data)
estimates = mod.inspect()

# Print model estimates
print(estimates)

# Show fit statistics
stats = semopy.calc_stats(mod)
print(stats.T)
```

The first output shows the model estimates, while the second one shows fit measures for the fitted model.

### Model estimates 

For a guide on how to interpret loadings, (co)variances and residuals, please refer to the previous chapter.

Lets focus on the newly added regression (`speed ~ visual`). The `Estimate` column can be refered as the slope of the added regression, meaning that a one unit increase in `visual` comes **on average** with a 0.37 unit increase in `speed`. As indicated by the `p-value`, this coefficient is significantly different from zero. With that, we can infer that `visual` is significantly predicting `speed`.

### Fit measures

To assess model fit, let's look at the fit measures (please refer to the previous chapter for details). The significant $\chi^2$-Test indicates that the model implied covariance matrix is significantly different from the empirical one. Furthermore, TLI (0.77), RMSEA (0.13) and CFI (0.88) are not in the desired windows, indicating bad fit.

