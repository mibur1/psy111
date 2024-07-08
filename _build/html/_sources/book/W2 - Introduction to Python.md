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

# W2 - Introduction to Python

The Python programming language is a *general purpose* language, which means it is widely used in many fields, including statistics and neuroimaging. Python is a *high-level, interpreted* language, which means it offers a high level of abstraction and you do not need to worry about *low-level* things like memory management. Its syntax is also relatively simple and aims to be readable.


## 1.1 Variables and data types

As in other programming languages, variables are used to store data of different values or types (hence the name *variable*). In Python, you declare a variable by writing its name and then assigning its value with the equal (=) sign:

```{code-cell}
my_variable = 4
```

Notice that when we initialize a variable, we do not need to specify its *type* (e.g. it being an integer or a string), as Python is *dynamically typed*, which means it figures out your desired variable type by itself once you run the program. This means, you can also simply overwrite your previously created variable with for example a character string:

```{code-cell}
my_variable = "Hello World"
```

If you want to know what 's currently stored in your variable, you can use the `print()` function:

```{code-cell}
print(my_variable)
```

**Note:** If you are working in an interactive environment like the Jupyter notebooks which we will use in the exercises, you might not even need to write print as the last line of the code cell is automatically evaluated.

## 1.2 Built-in types

All general-purpose languages provide the programmer with different *types* of variables as the basic building blocks programs are made up.