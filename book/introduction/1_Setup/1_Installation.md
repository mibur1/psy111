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

# 1.1 Installation

Python is a open source programming language that can be freely downloaded and installed from its official website. However, in this guide, we will use Python through a software package called **Conda**. Conda is a powerful package and environment management system that is widely used in the Python community and beyond.

The key advantage of using Conda is its ability to manage multiple versions of Python and various dependencies simultaneously. Conda allows you to create isolated environments, which you can think of as self-contained "boxes" that contain specific versions of Python and other packages required for different projects. This isolation ensures that changes or updates in one environment do not affect others, making it easier to manage and switch between projects with different requirements.

```{figure} ../../../_static/figures/conda.png
---
width: 70%
name: arrays
---
Version management with Conda.
```

We will use a minimal version of Conda called **Miniconda**. This is a lightweight version of Conda that, unlike its bigger sibling **Anaconda**, comes without e.g. a graphical user interface. You can download and install it from [here](https://docs.anaconda.com/miniconda/). Scroll down and select the suitable version for your operating system in the table. When installing Miniconda, please check the "Install for: All Users" checkbox to avoid potential issues with accessing Python packages.

In addition to Conda, we will also install and download a programming environment. For this, we use **Visual Studio Code**, which you can download from [here](https://code.visualstudio.com/). In addition to the default installation settings, we recommend you to check the "Open with code" checkboxes for easier usability later on.


```{admonition} Using Python through Conda
:class: tip

By using Conda you can:

- Install and switch between different versions of Python
- Manage dependencies for different projects separately, avoiding conflicts
- Create reproducible environments that can be easily replicated by others
```
