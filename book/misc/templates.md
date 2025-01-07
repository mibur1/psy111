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

# Micellaneous templates for MyST syntax

Inline directive to refer to a document: {doc}`overview`.

Citing: {cite}`Rokem2023`.

Code cells with a directive like so:

```{code-cell}
print(2 + 2)
```

Boxes. Possible classes are:
- blue: note, important
- green: hint, seealso, tip
- yellow: attention, caution, warning
- red: danger, error

```{admonition} Summary
:class: tip

Hello world
```

**The schedule for the semester will be:**

```{tableofcontents}
```

**Admonitions in .ipynb files**

:::{admonition} Always Filter First
Filtering should always be one of the first preprocessing steps you apply to your data. :::


## <i class="fas fa-book fa-fw"></i> Assessing Performance

```{margin}
{{ref_test}}\. This is a ref:

$y_i = w_0 + w_1x_i + \varepsilon_i$
```

This is how I use it in text<sup>{{ref_test}}</sup>

I can also add vides

```{video} ../video.mp4
```

This is empty space:

$\ $

This is just a plot without the code cell:

```{code-cell} ipython3
---
tags:
    - "remove-input"
mystnb:
  image:
    width: 400px
    alt: Joint distribution of house square footage (x-axis) and house price (y-axis) with marginal distributions on the side
    align: center
  figure:
    name: joint-distq
---

import pandas as pd
import numpy as np
import seaborn as sns

NUM_POINTS = 1000
SEED = 43
X_SCALE = 2000
Y_NOISE_SCALE = 60000

def f(x):
    return 100 + (0.5 * x + 1.5 * x * x) / 100

np.random.seed(SEED)
x = np.random.randn(NUM_POINTS) + 2
x = np.clip(x, 1, 20)
x = X_SCALE * x
y = f(x) + np.random.randn(NUM_POINTS) * Y_NOISE_SCALE
y = y + abs(y.min()) + 10000
df = pd.DataFrame({"Sq. Ft.": x, "Price": y})

_ = sns.jointplot(data = df, x = "Sq. Ft.", y = "Price", kind="kde")
```
