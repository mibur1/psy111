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


# 13.3 Summary

## Transform predictors

```{code-block}
# Transform predictor variable
transformed_x = dmatrix("bs(X, knots=(knot1, knot2, ...), degree=degree, include_intercept=False)",
                        {"X": df['X']}, return_type='dataframe')
```

- `X` is the predictor.
- `knots` specifies the cut points of the model.
- `degree` determines the degree of the fitted polynomials.

## Fit a model 

```{code-block}
# Fit the model
model = sm.GLM(data['Y'], transformed_x).fit()

# Print the model summary
print(model.summary())
```

- `Y` is the dependent variable.


## Plot the model

```{code-block}
plt.figure(figsize=(10, 6))
xp = np.linspace(df['X'].min(), df['X'].max(), 100)
transformed_xp = dmatrix("bs(xp, knots=(knot1, knot2, ...), degree=degree, include_intercept=False)",
                         {"xp": xp}, return_type='dataframe')

pred = model.predict(transformed_xp)

sns.scatterplot(x=df['X'], y=df['Y'], alpha=0.5, label='Data')
plt.plot(xp, pred, label='label', color='red')
plt.title("Fit")
plt.xlabel("X")
plt.ylabel("Y")
plt.legend().remove()
plt.show()
```
