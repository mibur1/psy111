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

# 2.1 Variables

As in other programming languages, variables are used to store data of different values or types (hence the name *variable*). In Python, you declare a variable by writing its name and then assigning its value with the equal (=) sign:

```{code-cell}
my_variable = 4
```

When naming variables, there are a few rules to keep in mind:
- Variable names must start with a letter or an underscore (_), but not a number.
- They can only contain letters, numbers, and underscores.
- Names are case-sensitive (`my_Variable` and `my_variable` are different).
- It's a good practice to use descriptive names and follow the snake_case convention (e.g., my_variable).

Notice that when we initialize a variable, we do not need to specify its *type* (e.g. it being an integer or a string), as Python is *dynamically typed*, which means it figures out your desired variable type by itself once you run the program. This means, you can also simply overwrite your previously created variable with for example a character string:

```{code-cell}
my_variable = "Hello World"
```

If you want to know what 's currently stored in your variable, you can use the `print()` function:

```{code-cell}
print(my_variable)
```

**Note:** If you are working in an interactive environment like the Jupyter notebooks which we will use in the exercises, you might not even need to write print as the last line of the code cell is automatically evaluated.

```{admonition} Summary
:class: tip

Variables in Python work like in most other programming languages:

- You assign values to them by using the equal (=) operator
- Python is dynamically typed; it figures out the variable type for you
- You can print their content with the `print()` function
```