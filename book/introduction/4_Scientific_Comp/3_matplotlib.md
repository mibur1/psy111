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

Both ways of importing sub-modules from a library are equivalent and you will encounter both of them in the wild. Then, in its simplest form, you can create a plot by simply calling the corresponding function:


```{code-cell}
plt.plot([1,2,3,5,7,9])
plt.show()
```
