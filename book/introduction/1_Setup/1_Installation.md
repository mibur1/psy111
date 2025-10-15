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

Python is an open-source programming language that can be freely downloaded and installed from its official website. In this guide, however, we will use Python through an environment manager called **Miniforge**. Miniforge contains `conda`, which allows you to install specific Python versions and manage project dependencies in isolated environments. You can think of these environments as self-contained “boxes” that include everything a project needs: a particular Python version and the required packages. This isolation ensures that updates or changes in one environment do not affect others, making it easy to manage and switch between projects with different requirements.


```{figure} ../../../_static/figures/miniforge.png
---
width: 70%
name: miniforge
---
Version management with Miniforge/Conda.
```

## Miniforge/Conda

**Linux/MacOS**

Open a terminal in the folder where your `.sh` script is located and run the script (*hint: after typing `bash Miniforge3` you should be able to hit `Tab` to autocomplete the command*):

```
bash Miniforge3-$(uname)-$(uname -m).sh
```

After a succesful installation, simply open a new terminal. You should now see a `(base)` in the beginning of each line, which tells you that the installation was succesful.

**Windows**

On Windows, simply run the `.exe` file and install everything. After installation, simply open the `Miniforge Prompt` from the start menu.


## Visual Studio Code

In addition to Miniforge, we will also install and download a programming environment. For this, we use **Visual Studio Code**, which you can download from [here](https://code.visualstudio.com/). In addition to the default installation settings, we recommend you to check the "Open with code" checkboxes for easier usability later on.


```{admonition} Using Python through a Environment Manager
:class: tip

By using an environment manager such as Miniforge/Conda you can:

- Install and switch between different versions of Python
- Manage dependencies for different projects separately, avoiding conflicts
- Create reproducible environments that can be easily replicated by others
```
