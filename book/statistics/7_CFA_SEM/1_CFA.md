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

# 11.1 CFA

Today, we will work with the `HolzingerSwineford1939` dataset, which contains mental ability test scores from seventh- and eighth-grade pupils in two schools. We focus on nine observed variables:

* **x1, x2, x3**: visual ability
* **x4, x5, x6**: text processing skills
* **x7, x8, x9**: speed ability

Like in the [Path Modelling](../5_Path_Modelling/1_Semopy) session, will use the `semopy` package. However, we will use a new operator `=~`, which allows us to define latent variables directly via their observed indicators, making CFA and SEM models easy to specify and read.

We first load and inspect the data:

```{code-cell} ipython3
import semopy

data = semopy.examples.holzinger39.get_data()
data
```

We can then simply fit a CFA model by using the standard string-based syntax to define three latent variables (visual, text and speed):

```{code-cell} ipython3
desc = '''visual =~ x1 + x2 + x3
          text =~ x4 + x5 + x6
          speed =~ x7 + x8 + x9'''

model = semopy.Model(desc)
results = model.fit(data)
print(results)
```

## Model estimates

We can inspect the estimated model parameters using:

```{code-cell} ipython3
estimates = model.inspect(std_est=True)
print(estimates)
```

### Loadings

The first nine rows correspond to the factor loadings, indicating how strongly each observed variable relates to its latent factor.

- One loading per latent variable is fixed to 1 in the unstandardised solution in order to identify (scale) the factor.
- The `Estimate` column contains the unstandardised loading.
- The `Est. Std` column contains the standardised loading.
- `Std. Err` represents the standard error (uncertainty) of the unstandardised estimate.
- `z-value` is the unstandardised estimate divided by its standard error.
- `p-value` tests the null hypothesis that the parameter equals zero in the population.
- Large absolute z-values and small p-values indicate that a loading is reliably different from zero.

Standardised loadings (`Est. Std`) can be interpreted similarly to regression coefficients or correlations. For example, a standardised loading of 0.77 indicates that an increase of one standard deviation in the latent factor is associated with an increase of 0.77 standard deviations in the observed variable. Standardised loadings thus are particularly useful for comparing the relative strength of indicators within and across factors.

### Variances and Covariances of Latent Variables

Rows such as `visual ~~ visual`, `text ~~ text`, and `speed ~~ speed` represent the variances of the latent variables.
Because all latent variables in this CFA model are exogenous (they are not predicted by other variables), these parameters are interpreted as variances.

- In the unstandardised solution, these values depend on the scaling of the latent variables.
- In the standardised solution, all latent variances are equal to 1 by definition.

Rows such as `visual ~~ text`, `visual ~~ speed`, and `text ~~ speed` represent covariances between latent variables, reflecting how strongly the constructs are associated with each other. The positive and statistically significant covariances indicate that the latent abilities are positively related under the specified model.

- In the unstandardised solution, these are raw covariances.
- In the standardised solution, these values correspond to latent correlations.

### Residual Variances of Observed Variables

The final nine rows correspond to the residual variances of the observed variables. The standardised column shows the standardised residual variance, that is, the proportion of variance in the observed variable that is not explained by the latent factor.

For example, a standardised residual variance of 0.40 means that 40% of the variance in the observed variable remains unexplained, while 60% is explained by the latent factor.

In practice, residual variances are almost always greater than zero, because latent variables rarely account for all variability in observed measures. Their presence reflects measurement error and construct-irrelevant variance.


```{admonition} Learning break
:class: note

1. How can you calculate the z-value yourself? 
2. When should you read a `variable ~~ variable` output as variance? When instead as residual variance?
```

<details>
<summary><strong>Click to show solution</strong></summary>

1. The z-value is computed as the Estimate divided by the SE.
2. Whether `variable ~~ variable` is interpreted as variance or residual variance depends on whether the variable is predicted in the model:
    - If the variable is exogenous (no incoming regression paths) it is the variance
    - If the variable is endogenous (it has predictors), it is the residual variance


## Fit measures

To assess model fit, `semopy` provides us with a wide range of fit measures:

```{code-cell} ipython3
stats = semopy.calc_stats(model)
print(stats.T)
```

- **χ² test (`chi2`, `chi2 p-value`)**  
  The chi-square test evaluates the null hypothesis that the model-implied covariance matrix equals the observed covariance matrix.
  A significant p-value indicates that the model does not reproduce the observed covariances exactly.
  However, the χ² test is highly sensitive to sample size, and significant results are common even for reasonably fitting models.

- **CFI (Comparative Fit Index)**  
  CFI compares the specified model to a baseline model that assumes no relationships among variables. Values closer to 1 indicate better fit.
  A CFI of 0.93 suggests an acceptable to good fit.

- **TLI (Tucker–Lewis Index)**  
  TLI is similar to CFI but penalises model complexity. Values closer to 1 indicate better relative fit.
  A TLI of 0.90 is often considered acceptable, though values above 0.95 are sometimes recommended.

- **RMSEA (Root Mean Square Error of Approximation)**  
  RMSEA summarises the degree of model misfit per degree of freedom, adjusting for model complexity.
  Values below 0.08 are often interpreted as acceptable, and values below 0.05 as good. An RMSEA of 0.09 indicates a mediocre fit.

- **Log-likelihood (`LogLik`)**  
  The log-likelihood reflects how likely the observed data are under the specified model and is used to compute information criteria.

- **AIC and BIC**  
  The Akaike Information Criterion (AIC) and Bayesian Information Criterion (BIC) balance model fit and complexity.
  Lower values indicate a better trade-off between goodness of fit and parsimony. These measures are useful only for comparing models, not as absolute indicators of fit.


## Visualizing the Model

For visualization, we can plot our model specified model using the following code.

```{code-cell} ipython3
semopy.semplot(model, plot_covs = True, std_ests=True, filename='data/cfa_plot.pdf')
```

## Comparing Models

Next to evaluating our main model using model fit measures, we can also compare it to another model. In the initial model, the latent factors are assumend to covary. However, a model, in which the latent factors are set to be independent might provide a better fit. To specify such a model we need to set the covariances between the factors to be zero.

```{code-cell} ipython3
desc2 = '''visual =~ x1 + x2 + x3
           text =~ x4 + x5 + x6
           speed =~ x7 + x8 + x9
           
           # Set covariance to zero
           speed ~~ 0 * visual
           speed ~~ 0 * text
           text ~~ 0 * visual'''

# Fit the model
model2 = semopy.Model(desc2)
results2 = model2.fit(data)

# Print results
estimates2 = model2.inspect(std_est=True)
print(estimates2)
```

```{code-cell} ipython3
# Print fit statistics
stats2 = semopy.calc_stats(model2)
print(stats2.T)
```

```{code-cell} ipython3
# Visualise the model
semopy.semplot(model2, std_ests=True, filename='data/cfa_plot2.pdf')
```

We can see that the covariances between the latent factors (e.g. speed  ~~  visual) are now forced to be zero.

```{code-cell} ipython3
print("Model1:")
print(stats.T)  # Model 1 (correlated latent factors)
print("\nModel2:")
print(stats2.T) # Model 2 (independent latent factors)
```

By comparing AIC and BIC values across models, we can assess which model provides a more parsimonious description of the data. Lower AIC and BIC values favour the simpler independence model. However, statistical fit must always be interpreted in light of theoretical considerations: a model with better information criteria is not necessarily the most meaningful from a substantive perspective.
