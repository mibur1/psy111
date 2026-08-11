---
kernelspec:
  name: python3
  display_name: Python 3
---

# Polynomial Regression

Polynomial regression extends the linear regression model by including higher-degree terms of a predictor variable. This allows the model to capture curved relationships between a predictor and an outcome, while retaining the core idea of estimating coefficients within a regression framework. The general form of a polynomial regression model of $n$-th degree is:

 $$\hat{y} = b_0 + b_1 x + b_2 x^2 + \dots + b_n x^n$$

The following plot shows some example data and allows you to fit polynomial models of varying degree to the data. Please take some time to explore these models:

```{code-cell} ipython3
:tags: [remove-input]
import warnings
import numpy as np
import pandas as pd
import plotly.graph_objects as go
import plotly.io as pio

# Fitting polynomials of up to degree 30 to 30 points is deliberately
# ill-conditioned - that is the point of the demo - so numpy's RankWarning is
# expected. Note it must be filtered by *category*; `message=` does not work.
try:
    RankWarning = np.exceptions.RankWarning   # numpy >= 1.25
except AttributeError:
    RankWarning = np.RankWarning              # numpy < 1.25
warnings.filterwarnings("ignore", category=RankWarning)

# A neutral Plotly look that stays legible in both the light and the dark
# version of the site: transparent paper, faint plot background, grey text.
psy111 = pio.templates["plotly_white"]
psy111.layout.paper_bgcolor = "rgba(0,0,0,0)"
psy111.layout.plot_bgcolor = "rgba(128,128,128,0.08)"
psy111.layout.font.color = "#888888"
pio.templates["psy111"] = psy111
pio.templates.default = "psy111"

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
fig
```

```{note} Learning break

What happens if you increase the order of the polynomial? Can you observe an interesting behavior?

*Hint: The data consists of 30 observations.*
```

:::{dropdown} Click to show solution
As you probably expected, the explained variance ($R^2$) increases as the order of the polynomial increases, eventually reaching 1 for a model of degree 29. At this point, the model passes exactly through every single data point.

This does not happen by chance. Any dataset with $n$ observations can be perfectly interpolated by a polynomial of degree  $n−1$. However, such a high-order model is usually not a good choice in practice, as it was *overfit* to the data and will generalise poorly to new observations. Although this will be more thoroughly discussed in the [psy112 module](https://mibur1.github.io/psy112) next semester, we will already cover it a bit in today's session.
:::
