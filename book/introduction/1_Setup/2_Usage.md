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

# 1.2 Usage

At this point you should have previously installed Python (through miniconda) and Visual Studio Code.


## The Python interpeter

In your programs, search for the `Anaconda Powershell Prompt (miniconda3)` and open it. If you are on macOS or Linux, open the normal terminal. In all cases, you should see a `(base)` indicating that we have succesfully installed Miniconda and are in our base environment.

First, we will open the Python interpreter of our current base environment by typing `python` and then presssing enter. The Python interpeter will start and show you the current Python version, for example:

```
Python 3.12.4 | packaged by Anaconda, Inc. | (main, Jun 18 2024, 15:12:24) [GCC 11.2.0] on win32
Type "help", "copyright", "credits" or "license" for more information.
>>>
```

Simply put, the Python interpreter is the program that executes your Python code. It translates the written code into a form that the computer can understand and then runs it. Inside the interpreter we can write any kind of Python code, like printing a message:

```
>>> print("Welcome to psy111!")
Welcome to psy111!
```

While this works for simple commands, it is not really useful for more complicated tasks. These would usually be implemented in self-contained Python scripts or Jupyter notebooks. For now, let us exit the Python interpreter by typing `quit()` and pressing enter.


## Conda environments

Remember that we use Conda to have isolated Python environments for our projects. So let's start by creating a **psy111** environment that is able to run all upcoming exercises (as well as create this entire book). Start by typing

```
conda create -n "psy111"
```

and confirm the installation with `y` when prompted to do so. Afterwards, activate the environment by typing

```
conda activate psy111
```

The `(base)`should now be changed to `(psy111)`, indicating that you have succesfully activated the new environment. In this environment we first install `pip`, which is the standard Python package manager:

```
conda install pip
```

You can then type

```
pip list
```

to display a list of all installed Python packages. As our environment is still new, this list is quite short. However, we are now ready to install any kind of Python package. For example, we can install NumPy, a very important package for numerical calculations in Python by typing

```
pip install numpy
```

If you then again type `pip list`, you will see that the list of installed packages now includes the NumPy. There are more things you can do with Conda, and if needed, you can refer to the [cheatsheet](https://docs.conda.io/projects/conda/en/4.6.0/_downloads/52a95608c49671267e40c689e0bc00ca/conda-cheatsheet.pdf) for more information.


## Python Scripts

We have previously used the Python interpreter to run simple Python code. However, you will ususally require a more organized way of running your code. For this, let's open Visual Studio Code.

```{figure} ../../../_static/figures/vscode.png
---
width: 100%
name: vscode
---
Visual Studio Code.
```

First, navigate to the "Extensions" button in the left column and install the **Python** extension. Then, in the top left click on "File" -> "New file..." and select "Python file". You will see that an empty file called "Untitled-1" was opened. We can save it by again clicking on "File" -> "Save", or by pressing Ctrl + S (or Command + S on Mac). If you previously selected "Python file", you can see that the `.py` file extension was automatically added to the file name (otherwise you can always do it manually).

You are now ready to write your Python script. As in the previous example, you can write

```
print("Welcome to psy111!")
```

and then run the Python script by pressing the run button at the top right. If you do this for the first time, VS Code will most likely promt you to select your Python interpeter at the bottom right. Here, you should now be able to see and select the previously created `psy111` enviroment. If the terminal displays a yellow button even after loading the environment, hover your mouse over it and click "reload terminal".


## Jupyter notebooks

Python scripts (like the one you created in the previous section) are single files that run from top to bottom. Sometimes, for example if you have small projects that you would like to share with other, you might want to only have smaller blocks of code with text or images between them. In such cases, you can use Jupyter notebooks. These files contain cells for Python code as well as for text, so you can neatly format and share your code with others. We will also use these noteboks for the exercises in this seminar.

```{figure} ../../../_static/figures/notebooks.png
---
width: 100%
name: notebooks
---
Jupyter Notebooks.
```

To be able to use Jupyter notebooks, you need to install the **Juypter** extension (just like the previously installed Python extension). You can then create and use `.ipynb` files, which is short for "interactive python notebook". Once you created such a notebook, you can add code cells with the `+ Code` and text cells with the `+ Markdown` buttons. The Python interpreter can again be selected from the `psy111` environment by pressing the "Select Kernel" button at the top right of the script.

### Code cells

Code cells can have any kind of Python code as an input. They will further also automatically print the last line:

```{code-cell}
a = 1
b = 2
a + b
```

### Markdown cells

Markdown is a way of writing and formatting text. In fact, all the text in this book is written in Markdown (or to be more specific with MyST Markdown, which is an extension to Markdown). As a start, you can use the [cheatsheet](https://www.markdownguide.org/cheat-sheet/) to learn about the basic syntax. You will be able to explore this in the upcoming exercises.


## The psy111 book

To be able to use the book on your local computer, you first need to download its source code from [GitHub](https://github.com/mibur1/psy111) by clicking on the green button and selecting "Download ZIP" (f you are familiar with Git, you can also clone the repository for easier updating in the future). Then, navigate to your Downloads, extract the psy111 folder.

```{admonition} File management
:class: caution

It is strongly recommend that you create and maintain an organized folder structure for your Master's course, and not keep everything unorganized in your Downloads folder!
```

Once you have saved and unpacked the psy111 folder, you can open it in VS Code. You can do so by right clicking in the folder and selecting "Open with Code" if you have previously checked the corresponding box in the installation process, or by clicking "File" -> "Open Folder" inside VS Code.

In the file overview on the left you should see a `requirements.txt` file. If you open it, you can see the necessary Python libraries that are needed for the book. You can install them by either manually typing them into the **Miniconda Promt**, or by using the Promt to navigate in the folder of the book with the `cd` (change directory) command and then installing from the requirements file itself:

```
cd path/to/your/book
pip install -r requirements.txt
```

You can then simply type

```
jb build .
```

to build the entire book on your computer. Once the build is finished, a link will show up in the terminal, which you paste into your browser to open the book (or you can just open the `_build/html/index.html` file manually). You can then modify the book in any way you like and rebuild it to show the changes.

You can also use the exercises locally by simply opening the `.ipynb` files from the relevant folders whithin the `book/` folder in VS Code. However, you can can also open them in Google Colab by opening them from whithin the book and clicking on the rocket symbol.

```{admonition} Summary
:class: tip

You can use Python in different ways:

- Through standard Python scripts (`.py` files)
- Through interactive Jupyter Notebooks (`.ipynb` files)

You can use Conda to manage your Python environments. You can:

- Create environments with `conda create -n <env_name>`
- Switch environments with `conda activate <env_name>`
- Install packages using `pip install` inside the environment
```
