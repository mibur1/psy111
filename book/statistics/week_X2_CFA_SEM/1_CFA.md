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

To compute an CFA in Python we will again use the `factor_analyzer` package. 

### Example dataset

For todays example, a dataset form the `sklearn` package is used. This dataset contains different parameters that describe breast tumors. For a detailed description, see [sklearn documentation](https://scikit-learn.org/stable/datasets/toy_dataset.html#breast-cancer-dataset). 

```{code-cell}
# Load and inspect the dataset 
from sklearn.datasets import load_wine, load_breast_cancer
import pandas as pd

data_cancer = load_breast_cancer()
df_cancer = pd.DataFrame(data_cancer.data, columns=data_cancer.feature_names)
print(df_cancer.head())
```

### Specify the model

We assume there to be 2 factors. One factor that represents size-related attributes of the tumor, one factor that represents the texture of the tumor.

```{code-cell}
# 
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