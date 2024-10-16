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

# X2.1 CFA in Python 

To compute an CFA in Python we will use the `semopy` package. 

### Example dataset

Today, we use the `HolzingerSwineford1939` dataset. The dataset contains mental ability test scores of seventh and eighth grade children from two different schools. Apart from demographic information, nine of the original 26 tests are included in the data set.

- x1, x2 and x3 are indicators for visual ability
- x4, x5 and x6 are indicators for text processing related skills
- x7, x8 and x9 are indicators for speed ability

```{code-cell}
# Load and inspect the dataset 
from semopy.examples import holzinger39
data = holzinger39.get_data()
print(data)
```

### Specify and fit the model

Let us use the `semopy` syntax to define three latent variables - visual, text and speed.

```{code-cell}
# Specify the model
desc = '''visual =~ x1 + x2 + x3
text =~ x4 + x5 + x6
speed =~ x7 + x8 + x9'''

# Fit the model
mod = Model(desc)
res_opt = mod.fit(data)
estimates = mod.inspect()

# Print model estimates
print(estimates)

# Show fit statistics
stats = semopy.calc_stats(mod)
print(stats.T)
```

The first output shows the model estimates, while the second one shows fit measures for the fitted model.

#### Model estimates 

- Loadings: The `Estimate` collumn for the first 9 lines represents the loadings of the 9 measured variables on the 3 factors. You may notice that one loading per factor is set to 1. This is done to identify the factor (see lecture for details). The `Std. Err` collumn shows the uncertainty associated with the estimate. The `z-value` represents how many standard deviation the estimate is away from zero. The last column `p-value` contains the p-value (probability) for testing the null hypothesis that the parameter equals zero in the population.

- Variances: Lines 9 (speed  ~~   speed), 12 and 13 show the variances of the respective latent factors.

- Covariances: The lines 10 (speed  ~~    text), 11, 14 show the covariances, e.g. the associations between the latent variables. Since all estimates are positive and significantly different from zero (see `p-value`), we can infer that the latent factors are positively associated with each other. 

- Residual Variances: The last 9 lines show the residual variances of the measured variables. Remember, in CFA/SEM we aim at finding latent variables that explain variance in measured variables. However, most of the times, the latent variables can't account for 100% of the variance in a measured variable. In fact, as all residual variances are significantly different from zero (see `p-value`), we can infer that there's is still a significant amount of variance in each measured variable that is not explained by the respective latent factor.

```{admonition} Use your own brain!
:class: note

How can you calculate the z-value yourself?
```

#### Fit measures

To assess model fit, `semopy` provides us with a wide range of fit measures. Let's interpret the ones we know from the lecture.

- `chi2` / `chi2 p-value`: The $\chi^2$-Test tests the null hypothesis that the model implied covariance matrix is equal to the empirical (actual) covariance matrix. Therefore, a **low** test statistic (and a non-significant p-value) indicate good fit. In this case, the p-value is <.05, meaning that the model’s predicted covariance matrix significantly differs from the observed covariance matrix, indicating that the model might not adequately capture the relationships in your data. However, the test statistic of the baseline model (assuming no relationships between the variables, i.e. the worst possible model) is much higher, indicating our model is better than the baseline model.

- `CFI`: The CFI compares the fit of your user-specified model to the baseline model, with values closer to 1 indicating that the user model has a much better fit. A CFI of 0.931 suggests a good model fit.

- `TLI`: Similar to CFI, TLI also compares your model to the baseline model, penalizing for model complexity. A value close to 1 indicates that your user model has a better fit than the baseline model. TLI of 0.896 is reasonably good, though slightly below the preferred threshold of 0.95.

- `RMSEA`: The RMSEA can be seen as a rescalled version of the $\chi^2$-Test which is not dependent on sample size (as the $\chi^2$-Test is). RMSEA value of <0.08 indicate good fit. Here, RMSEA =  0.092 indicates a mediocre fit. 

- `LogLik`: These are used to compute information criteria. They represent the likelihood of the model given the data.

- `AIC`: A measure of the relative quality of the statistical model for a given set of data. Lower AIC values indicate a better model. This statistic can be only used for comparison but not as an absolute criterion. 

- `BIC`:  Similar to AIC, but includes a penalty for the number of parameters in the model. Lower BIC values indicate a better model. The sample-size adjusted BIC is more appropriate for smaller sample sizes. Also similar to the AIC, the BIC is only used for model comparison.



### Plot the model

For visualization, we can plot our model specified model using the following code.

```{code-cell}
# Plot the model
from semopy import semplot
mod_plot = semplot(mod, filename='mod_plot.png')
mod_plot
```



### Specify and fit an alternative model

Next to evaluating our main model using model fit measures, we can also compare it to another model. In the initial model, the latent factors are assumend to covary. However, a model, in which the latent factors are set to be independent might provide a better fit. To specify such a model we need to set the covariances between the factors to be zero.

```{code-cell}
# Specify the model
desc2 = '''visual =~ x1 + x2 + x3
text =~ x4 + x5 + x6
speed =~ x7 + x8 + x9
# Set covariances to zero
speed ~~ 0 * visual
speed ~~ 0 * text
text ~~ 0 * visual'''

# Fit the model
mod2 = Model(desc2)
res_opt2 = mod2.fit(data)
estimates2 = mod2.inspect()

# Print model estimates
print(estimates2)

# Show fit statistics
stats2 = semopy.calc_stats(mod2)
print(stats2.T)
```

As you can see, the covariances between the latent factors (e.g. speed  ~~  visual)are forced to be zero. Please refer to the first model for the model interpretation.

### Compare models

To see which of our models provides a better fit, we can compare them. For that, lets print the model fit measures for both models again.

```{code-cell}
# Model 1 (non-independent latent factors)
print(stats.T)

# Model 2 (independent latent factors)
print(stats2.T)
```

Let's begin by comparying AIC and BIC. As stated above a **lower value** indicates a **better** fit (for AIC and BIC). Here, AIC and BIC both favor the simpler model that assumes indepence between the latent variables. 

To further compare the models, we can compare them using a test. 