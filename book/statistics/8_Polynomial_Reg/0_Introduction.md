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

# Polynomial Regression

Polynomial regression extends the linear regression model by including higher-degree terms of a predictor variable. This allows the model to capture curved relationships between a predictor and an outcome, while retaining the core idea of estimating coefficients within a regression framework. The general form of a polynomial regression model of $n$-th degree is:

 $$\hat{y} = b_0 + b_1 x + b_2 x^2 + \dots + b_n x^n$$

The following plot shows some example data and allows you to fit polynomial models of varying degree to the data. Please take some time to explore these models:

```{code-cell} ipython3
:tags: [remove-input]
from IPython.display import HTML
import plotly.io as pio
import numpy as np
import pandas as pd
import plotly.graph_objects as go
import warnings

warnings.filterwarnings("ignore", message=".*Polyfit may be poorly conditioned.*")

# Helper function to return a Plotly figure as self-contained HTML.
def show_plotly(fig, include_js='cdn'):
    return HTML(pio.to_html(fig, full_html=False, include_plotlyjs=include_js))

# --- Data ---
x = np.linspace(-5, 5, 30)
y = (x**3 + np.random.normal(0, 15, size=x.shape)) / 50
df = pd.DataFrame({'x': x, 'y': y})

# Scatter trace
scatter = go.Scatter(
    x=df['x'], y=df['y'],
    mode='markers',
    marker=dict(size=10, color='lightgrey', line=dict(color='gray', width=2)),
    name='Data'
)

# --- Regression curves for polynomial orders 0 through 30 ---
regression_traces = []
r2_list = []
x_fit = np.linspace(-5, 5, 400)

for order in range(0, 31):
    coeffs = np.polyfit(df['x'], df['y'], order)
    y_fit = np.polyval(coeffs, x_fit)

    trace = go.Scatter(
        x=x_fit, y=y_fit,
        mode='lines',
        name='Model',
        visible=False,
        line=dict(width=3, color='#4c72b0')
    )
    regression_traces.append(trace)

    y_pred = np.polyval(coeffs, df['x'])
    r2 = 1 - np.sum((df['y'] - y_pred) ** 2) / np.sum((df['y'] - df['y'].mean()) ** 2)
    r2_list.append(r2)

# Combine data: show scatter and 0th-order model initially
data = [scatter] + regression_traces
data[1]['visible'] = True  # order 0

# --- Slider steps ---
steps = []
for i in range(31):
    vis = [True] + [False] * 31
    vis[i + 1] = True

    step = dict(
        method="update",
        args=[
            {"visible": vis},
            {"annotations": [dict(
                x=0.5, y=0.98,
                xref="paper", yref="paper",
                text=f"R² = {r2_list[i]:.3f}",
                showarrow=False,
                font=dict(size=18, color="gray"),
                xanchor="center", yanchor="top"
            )]}
        ],
        label=str(i)
    )
    steps.append(step)

# Slider
sliders = [dict(
    active=0,
    currentvalue={"prefix": "Order of the polynomial model: "},
    pad={"t": 50},
    steps=steps
)]

# --- Layout ---
layout = go.Layout(
    annotations=[dict(
        x=0.5, y=0.98,
        xref="paper", yref="paper",
        text=f"R² = {r2_list[0]:.3f}",
        showarrow=False,
        font=dict(size=18, color="gray"),
        xanchor="center", yanchor="top"
    )],
    sliders=sliders,
    xaxis=dict(title="x", range=[-5.5, 5.5], tickfont=dict(size=14),
               fixedrange=True, gridwidth=1, zerolinewidth=1),
    yaxis=dict(title="y", range=[-3, 3], tickfont=dict(size=14),
               fixedrange=True, gridwidth=1, zerolinewidth=1),
    legend=dict(font=dict(size=14)),
    margin=dict(l=10, r=10, t=20, b=20),
)

# Figure
fig = go.Figure(data=data, layout=layout)
show_plotly(fig, include_js='cdn')
```

```{admonition} Learning break
:class: note

What happens if you increase the order of the polynomial? Can you observe an interesting behavior?

*Hint: The data consists of 30 observations.*
```

<details>
<summary><strong>Click to show solution</strong></summary>

As you probably expected, the explained variance ($R^2$) increases as the order of the polynomial increases, eventually reaching 1 for a model of degree 29. At this point, the model passes exactly through every single data point.

This does not happen by chance. Any dataset with $n$ observations can be perfectly interpolated by a polynomial of degree  $n−1$. However, such a high-order model is usually not a good choice in practice, as it was *overfit* to the data and will generalise poorly to new observations. Although this will be more thoroughly discussed in the [psy112 module](https://mibur1.github.io/psy112) next semester, we will already cover it a bit in today's session.
