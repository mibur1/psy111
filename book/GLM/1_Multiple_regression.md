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

# Multiple Regression
Multiple regression performs linear regression but with more than just one independent variable.

```{admonition} Independet and dependent
:class: tip

Dependent variable: The variable we qould like to explain by our model</br>
Independet variable: Variable(s) we use to explain the dependent variable  
```
There are a lot of different ways and packages you could use to perform multiple Regression in python. Some do it manually, some with th sklearn package and others with Statsmodel. Today we will focus on Statsmodel
$model = smf.ols(formula='Volume ~ Girth + Height', data=df).fit()
model.summary()$
Example of cars with the sklearn module
model = smf.ols(formula='Volume ~ Girth + Height', data=df).fit()
model.summary()

Before we decide what model fits best for the data, we visualize it:
```{code-cell}
 # Generate some data
 np.random.seed(12345)
 x = np.random.randn(100)*30
 y = np.random.randn(100)*10
 z=3+0.4*x + 0.05*y+10*np.random.randn(len(x))
 # Put them into a DataFrame
 df = pd.DataFrame({'x':x, 'y':y, 'z':z})
 # Show them
 fig, axs = plt.subplots(1,2)
 df.plot('x', 'z', kind='scatter', ax=axs[0])
 df.plot('z', 'y', kind='scatter', ax=axs[1])
 ```
 {image} 


