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

Python is an open-source programming language that can be freely downloaded and installed from its official website. In this guide, however, we will use Python through an environment manager called **Micromamba**. Micromamba allows you to install specific Python versions and manage project dependencies in isolated environments. You can think of these environments as self-contained “boxes” that include everything a project needs: a particular Python version and the required packages. This isolation ensures that updates or changes in one environment do not affect others, making it easy to manage and switch between projects with different requirements.

Apart from Micromamba, there are other ways to create Python environments, such as Conda or Miniconda. The main difference is that Micromamba is faster, lighter, and not subject to the recent [licensing restrictions](https://www.fz-juelich.de/en/rse/the_latest/the-anaconda-is-squeezing-us), which is why we recommend it here.


```{figure} ../../../_static/figures/mamba.png
---
width: 70%
name: mamba
---
Version management with Micromamba.
```

You can download and install Micromamba as described in the the [documentation](https://mamba.readthedocs.io/en/latest/installation/micromamba-installation.html). The easiest way is by typing the following command in your terminal and confirming all prompty by typing `y` (yes) and `Enter`:

On Linux/MacOS:

```
"${SHELL}" <(curl -L micro.mamba.pm/install.sh)
```

On Windows (PowerShell):

```
Invoke-Expression ((Invoke-WebRequest -Uri https://micro.mamba.pm/install.ps1 -UseBasicParsing).Content); 
Set-ExecutionPolicy -Scope CurrentUser -ExecutionPolicy RemoteSigned -Force
```

In addition to Micromamba, we will also install and download a programming environment. For this, we use **Visual Studio Code**, which you can download from [here](https://code.visualstudio.com/). In addition to the default installation settings, we recommend you to check the "Open with code" checkboxes for easier usability later on.


```{admonition} Using Python through a Environment Manager
:class: tip

By using an environment manager such as Micromamba you can:

- Install and switch between different versions of Python
- Manage dependencies for different projects separately, avoiding conflicts
- Create reproducible environments that can be easily replicated by others
```
