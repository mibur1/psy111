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

## Conda

You can download and install Conda trough Miniforge as described in the the [documentation](https://conda-forge.org/download/).

**Linux/MacOS**

Open a terminal in the folder where your `.sh` script is located and run the script (*hint: after typing `bash Miniforge3` you should be able to hit `Tab` to autocomplete the command*):

```
bash Miniforge3-$(uname)-$(uname -m).sh
```

After a succesful installation, simply open a new terminal. You should now see a `(base)` in the beginning of each line, which tells you that the installation was succesful.

**Windows**

On Windows, simply run the `.exe` file and install everything. We also recommend you to check the **"Add Miniforge3 to my PATH environment variable"** checkbox during installation. This will enable you to use `conda` from any terminal, and not just `Miniforge Prompt`, which will be installed in any case. Once the installation is finished, depending on your choice during the installation, you can either open your standard terminal or the `Miniforge Prompt`. In any case, you should then be greeted with a `(base)` in the beginning of the line, which means the installation was succesful and you are now in the base environment.


## Visual Studio Code

In addition to Miniforge, we will also install and download a programming environment. For this, we use **Visual Studio Code**, which you can download from [here](https://code.visualstudio.com/). In addition to the default installation settings, we recommend you to check the "Open with code" checkboxes for easier usability later on.


```{admonition} Using Python through a Environment Manager
:class: tip

By using an environment manager such as Miniforge/Conda you can:

- Install and switch between different versions of Python
- Manage dependencies for different projects separately, avoiding conflicts
- Create reproducible environments that can be easily replicated by others
```
