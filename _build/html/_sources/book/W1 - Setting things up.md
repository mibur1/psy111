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

# Setting things up

This book aims to be a standalone resource for the entire semester. Each session will start with a brief recap of the core concepts from the lecture and an introduction into the relevant Python tools. The second part will then let you apply the methods through hands-on exercises. These exercises can be done directly within the browser through [Google Colab](https://colab.research.google.com/), however the individual Jupyter Notebook files (`.ipynb`) can also be opened and run on your local computer.

As this will most definitely be useful for your future research, we will start with setting up a working Python environment on your own computer.

## Installation

Python can easily be installed and downloaded from the [official website](https://www.python.org/). However, in this guide, we will use a different package called [conda](https://conda.io/projects/conda/en/latest/user-guide/getting-started.html). Conda is a powerful package and environment management system that is widely used in the Python community and beyond.

The key advantage of using conda is its ability to manage multiple versions of Python and various dependencies simultaneously. Conda allows you to create isolated environments, which you can think of as self-contained "boxes" that contain specific versions of Python and other packages required for different projects. This isolation ensures that changes or updates in one environment do not affect others, making it easier to manage and switch between projects with different requirements.

We will use a minimal version of conda called **miniconda**. This is a lightweight version of conda that, unlike its bigger sibling, comes without e.g. a graphical user interface. You can download and install it from [here](https://docs.anaconda.com/miniconda/).


In addition to Python, we will also install and download a programming environment. For this, we use **Visual Studio Code**, which you can download from [here](https://code.visualstudio.com/).

```{important} Summary
:class: admonition-summary

By using conda/miniconda you can:

- Install and switch between different versions of Python
- Manage dependencies for different projects separately, avoiding conflicts
- Create reproducible environments that can be easily replicated by others
```

## Citations


Here is an inline directive to refer to a document: {doc}`notebooks`.


You can also cite references that are stored in a `bibtex` file. For example,
the following syntax: `` {cite}`Holdgraf2014` `` will render like
this: {cite}`Holdgraf2014`.

Moreover, you can insert a bibliography into your page with this syntax:
The `{bibliography}` directive must be used for all the `{cite}` roles to
render properly.
For example, if the references for your book are stored in `references.bib`,
then the bibliography is inserted with:

```{bibliography}
```

Jupyter Book also lets you write text-based notebooks using MyST Markdown.
See [the Notebooks with MyST Markdown documentation](https://jupyterbook.org/file-types/myst-notebooks.html) for more detailed instructions.
This page shows off a notebook written in MyST Markdown.

We can use these for introductory content that requires example code

## An example cell

With MyST Markdown, you can define code cells with a directive like so:

```{code-cell}
print(2 + 2)
```
