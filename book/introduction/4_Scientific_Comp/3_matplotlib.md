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

# 4.3 Matplotlib

A picture is worth a thousand words, and this holds especially true when working with complex data. Data, on its own, cannot tell its story, but through visualization, we can bring clarity to its patterns and insights. Effective data visualization is a critical part of the research process, allowing researchers to communicate findings clearly. Being a good researcher, therefore, also means being able to create compelling visual representations that make complex information accessible and understandable.

There are a few different Python software packages that help you with visualizing data, for example [matplotlib](https://matplotlib.org/), [seaborn](https://seaborn.pydata.org/), or [plotly](https://plotly.com/python/). Please feel free to explore the examples on the websites to see what is possible with these libraries.

For now, we will start with matplotlib, as it is probably the most widely used package out there (and also builds the foundation for e.g. seaborn). Matplotlib was first developed nearly 20 years ago by John Hunter, a postdoctoral researcher in neuroscience at the University of Chicago. Frustrated by proprietary tools for visualizing brain data, he created an open-source alternative. What started as a solo project has since grown into a widely used library across many fields, from visualizing NASA’s Mars landings to Nobel Prize-winning gravitational wave research, and of course, neuroscience data. One of Matplotlib’s strengths is its fine-grained control over the appearance of visualizations. Let’s start with the basics before diving into these details and install matplotlib through the Conda terminal (if you have previously installed the requirements of the Jupyter book it will tell you that it is already installed):

```
pip install matplotlib
```

Matplotlib is a very powerful library with several different interfaces. The one you should almost always use is the `pyplot` module, which you can import with any of the two following two lines of code:

```{code-cell}
from matplotlib import pyplot as plt
import matplotlib.pyplot as plt
```

Both ways of importing sub-modules from a library are equivalent and you will encounter both of them in the wild. Then, in its simplest form, we can create a plot by calling the `plt.subplots()` function:

```{code-cell}
fig, ax = plt.subplots()
```

You can see that you have created an empty plot. In its simplest form, the `subplots` function is called with no arguments, returning two objects: a Figure container for Axes (`fig`) and an Axes canvas for data (`ax`). The Figure acts as the overall page, holding and organizing multiple Axes objects, while the Axes is where the data is plotted.

## Line plots

To add data, we use methods of the `ax` object. For example, let's plot data from Harry Harlow's 1949 experiments {cite}`Harlow1949`, where animals chose between two options, one rewarded with a treat. Below are the results from three blocks of trials: the first, mid-experiment, and the last block.

```{code-cell}
trial = [1, 2, 3, 4, 5, 6]
first_block = [50, 51.7, 58.8, 68.8, 71.9, 77.9]
middle_block = [50, 78.8, 83, 84.2, 90.1, 92.7]
last_block = [50, 96.9, 97.8, 98.1, 98.8, 98.7]
```

As shown in the numbers, performance on the first trial of each block averaged 50%, since the animals had no prior knowledge of which option would be rewarded. After the initial trial, learning began, and their performance gradually improved. In the first block, improvement was slow and challenging, while in the final block, the animals showed rapid improvement. The middle blocks showed moderate progress, neither as slow as the first block nor as fast as the last. Harlow suggested this reflected the animals' ability to "learn to learn" by understanding the task's context—introducing the concept of a learning set. While this description provides some insight, a visual representation is far more revealing. Let’s recreate the graph from Harlow’s classic paper using the `ax.plot` method:

```{code-cell}
fig, ax = plt.subplots()
ax.plot(trial, first_block)
plt.show()
```

Calling `ax.plot` adds a line to the plot. The horizontal axis (x-axis) represents the trials within the block, while the vertical axis (y-axis) shows the average percent of correct responses for each trial. This line visualizes the gradual learning that occurs over the first set of trials.

If you'd like to include more data, such as additional trial blocks, you can simply add more lines to the plot. Let’s now see how we can add data for the other blocks to compare performance across them:


```{code-cell}
fig, ax = plt.subplots()

ax.plot(trial, first_block)
ax.plot(trial, middle_block)
ax.plot(trial, last_block)

plt.show()
```

With multiple lines, it quickyl becomes hard do distinguish them. We can improve this by simply adding a legend, labels, and a title:

```{code-cell}
fig, ax = plt.subplots()

ax.plot(trial, first_block, label="First block")
ax.plot(trial, middle_block, label="Middle block")
ax.plot(trial, last_block, label="Last block")

ax.legend()
ax.set(xlabel='Trials', ylabel='Percent correct', title='Harlow learning experiment')

plt.show()
```

Before we're done, there’s still some customization to improve the clarity of the plot. Right now, the data appears continuous, which is misleading since measurements were only taken at specific trials. We can fix this by adding markers to indicate where the measurements occurred. Each variable can have a different marker, added as keyword arguments in the `plot` call. We'll also set `linestyle='--'` for a dashed line to better reflect the discrete nature of the data:


```{code-cell}
fig, ax = plt.subplots()

ax.plot(trial, first_block, marker='o', linestyle='--', label="First block")
ax.plot(trial, middle_block, marker='v', linestyle='--', label="Middle block")
ax.plot(trial, last_block, marker='^', linestyle='--', label="Last block")

ax.legend()
ax.set(xlabel='Trials', ylabel='Percent correct', title='Harlow learning experiment')

plt.show()
```


## Scatter plots



## Statistical visualizations

Remember the date from the pandas section, which we loaded as a pandas data frame:

```{code-cell}
import pandas as pd
import seaborn as sns

yeatman_data = pd.read_csv("https://yeatmanlab.github.io/AFQBrowser-demo/data/subjects.csv",
                      usecols=[1,2,3,4,5,6,7],
                      na_values="NaN",
                      index_col=0)
print(yeatman_data.head())
```

```{code-cell}
sns.barplot(data=yeatman_data, x="Handedness", y="IQ", hue="Gender")
plt.show()
```
