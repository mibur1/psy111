# 1.1 Installation

Python can easily be installed and downloaded from the [official website](https://www.python.org/). However, in this guide, we will use a different package called [conda](https://conda.io/projects/conda/en/latest/user-guide/getting-started.html). Conda is a powerful package and environment management system that is widely used in the Python community and beyond.

The key advantage of using conda is its ability to manage multiple versions of Python and various dependencies simultaneously. Conda allows you to create isolated environments, which you can think of as self-contained "boxes" that contain specific versions of Python and other packages required for different projects. This isolation ensures that changes or updates in one environment do not affect others, making it easier to manage and switch between projects with different requirements.

```{figure} ../../../_static/figures/conda.png
---
width: 100%
name: arrays
---
Version management with conda.
```

We will use a minimal version of conda called **miniconda**. This is a lightweight version of conda that, unlike its bigger sibling, comes without e.g. a graphical user interface. You can download and install it from [here](https://docs.anaconda.com/miniconda/).

In addition to Python, we will also install and download a programming environment. For this, we use **Visual Studio Code**, which you can download from [here](https://code.visualstudio.com/). You can use the recommended settings for both installlations.

```{admonition} Summary
:class: tip

By using conda/miniconda you can:

- Install and switch between different versions of Python
- Manage dependencies for different projects separately, avoiding conflicts
- Create reproducible environments that can be easily replicated by others
```
