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

We start with importing the dataset:

```{code-cell}
import semopy

data = semopy.examples.holzinger39.get_data()
data
```

## Performing CFA

As for path modelling, we will use the `semopy` package for running our CFA. We can use the standard string-based syntax to define three latent variables (visual, text and speed) and fit the model:

```{code-cell}
desc = '''visual =~ x1 + x2 + x3
          text =~ x4 + x5 + x6
          speed =~ x7 + x8 + x9
          '''

model = semopy.Model(desc)
results = model.fit(data)
print(results)
```

We can then have a look at the estimates:

```{code-cell}
estimates = model.inspect()
print(estimates)
```

and fit measures:

```{code-cell}
stats = semopy.calc_stats(model)
print(stats.T)
```

### Model estimates

- **Loadings**: The `Estimate` column for the first 9 lines represents the loadings of the 9 measured variables on the 3 factors. You may notice that one loading per factor is set to 1. This is done to identify the factor (see lecture for details). The `Std. Err` column shows the uncertainty associated with the estimate. The `z-value` represents how many standard deviation the estimate is away from zero. The last column `p-value` contains the p-value (probability) for testing the null hypothesis that the parameter equals zero in the population.

- **Variances**: Lines 9 (`speed  ~~  speed`), 12 and 13 show the variances of the respective latent factors.

- **Covariances**: The lines 10 (`speed  ~~   text`), 11, 14 show the covariances, e.g. the associations between the latent variables. Since all estimates are positive and significantly different from zero (see `p-value`), we can infer that the latent factors are positively associated with each other.

- **Residual Variances**: The last 9 lines show the residual variances of the measured variables. Remember, in CFA/SEM we aim at finding latent variables that explain variance in measured variables. However, most of the times, the latent variables can't account for 100% of the variance in a measured variable. In fact, as all residual variances are significantly different from zero (see `p-value`), we can infer that there is still a significant amount of variance in each measured variable that is not explained by the respective latent factor.

```{admonition} Learning break
:class: note

1. How can you calculate the z-value yourself? 
2. When should you read a `variable ~~ variable` output as variance? When instead as residual variance?

```

### Fit measures

To assess model fit, `semopy` provides us with a wide range of fit measures. Let's interpret the ones we know from the lecture.

- `chi2` / `chi2 p-value`: The $\chi^2$-Test tests the null hypothesis that the model implied covariance matrix is equal to the empirical (actual) covariance matrix. Therefore, a **low** test statistic (and a non-significant p-value) indicate good fit. In this case, the p-value is <.05, meaning that there is a **significant misfit** (the model’s predicted covariance matrix significantly differs from the observed covariance matrix, indicating that the model might not adequately capture the relationships in your data). 
However, the test statistic of the baseline model (assuming no relationships between the variables, i.e. the worst possible model) is much higher, indicating our model is better than the baseline model - see CFI and TLI.

- `CFI`: The **CFI** compares the fit of your user-specified model to the baseline model, with values closer to 1 indicating that the user model has a much better fit. A CFI of 0.931 suggests a good model fit.

- `TLI`: Similar to CFI, **TLI** also compares your model to the baseline model, penalizing for model complexity. A value close to 1 indicates that your user model has a better fit than the baseline model. TLI of 0.896 is reasonably good, though slightly below the preferred threshold of 0.95.

- `RMSEA`: The **RMSEA** can be seen as a statistic derived from the $\chi^2$ test, adjusted for model complexity and less influenced by sample size. An RMSEA value of <0.08 indicates an adequate fit. In this case, RMSEA = 0.092 suggests a mediocre fit, above the commonly accepted threshold for good fit.

- `LogLik`: These are used to compute information criteria (AIC and BIC). They quantify the likelihood of observing the given the data under the specified model.

- `AIC`: A measure of the relative quality of the statistical model for a given set of data. Lower **AIC** values indicate a better model. This statistic can be only used for comparison but not as an absolute criterion.

- `BIC`:  Similar to AIC, but includes a penalty for the number of parameters in the model. Lower **BIC** values indicate a better model. The sample-size adjusted BIC is more appropriate for smaller sample sizes. Also similar to the AIC, the BIC is only used for model comparison.


### Visualizing the Model

For visualization, we can plot our model specified model using the following code.

```{code-cell}
semopy.semplot(model, plot_covs = True, filename='data/cfa_plot.pdf')
```

## Fitting an Alternative Model

Next to evaluating our main model using model fit measures, we can also compare it to another model. In the initial model, the latent factors are assumend to covary. However, a model, in which the latent factors are set to be independent might provide a better fit. To specify such a model we need to set the covariances between the factors to be zero.

```{code-cell}
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
estimates2 = model2.inspect()
print(estimates2)

stats2 = semopy.calc_stats(model2)
print(stats2.T)

# Visualise the model
semopy.semplot(model2, filename='data/cfa_plot2.pdf')
```

We can see that the covariances between the latent factors (e.g. speed  ~~  visual) are now forced to be zero.

## Compare models

To see which of our models provides a better fit, we can compare them. For that, lets print the model fit measures for both models again.

```{code-cell}
print(stats.T)  # Model 1 (correlated latent factors)
print(stats2.T) # Model 2 (independent latent factors)
```

We can compare model fits by looking at their AIC and BIC. As stated above **lower values** indicate a **better** fit. Here, AIC and BIC both favor the simpler model which assumes indepence between the latent variables.