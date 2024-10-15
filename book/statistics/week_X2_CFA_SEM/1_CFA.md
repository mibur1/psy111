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
data = holzinger39.get_data()
print(data)
```

### Specify the model

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

### Plot the model

For visualization, we can plot our model specified model using the following code.

```{code-cell}
# Plot the model
from semopy import semplot
mod_plot = semplot(mod, filename='mod_plot.pdf')
mod_plot
```



### Specify an alternative model

Next to evaluating our main model using model fit measures, we can also compare it to another model. For example, we can evaluate if the more complex 2-factor approach provides a significantly better fit than a less complex 1-factor model. One should always opt to go with the easiest model possible, therefore, if you have a complex model, also verify that is provides a significantly better fit than a less complex one.

```{code-cell}
# 
```

### Compare models

To see whether the more complex model provides a significantly better fit than the 1-factor model, we can compare them.

```{code-cell}
# 
```