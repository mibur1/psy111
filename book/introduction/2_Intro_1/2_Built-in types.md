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

# 2.2 Built-in types

All general-purpose languages provide the programmer with different *types* of variables as the basic building blocks of programs.

## Integers

Integers are the numbers zero (0), positive natural numbers (1, 2, 3, ...), or the negation of positive natural numbers (-1, -2, -3, ...)

```{code-cell}
subjects_group_1 = 10
subjects_group_2 = 20
scans_total = 60
```

You can perform mathematical operations like addition

```{code-cell}
subjects_total = subjects_group_1 + subjects_group_2
print(subjects_total)
```

or division

```{code-cell}
scans_per_subject = scans_total / subjects_total
scans_per_subject
```

*Note:* As already mentioned above, if we execute Python code in interactive notebooks (*.ipynb* files), the `print()` statement can be omitted if the variable to be printed is in the last line of the code block. We will use both options from time to time, as the automatic printing will perform some automatic formatting and sometimes one or the other will look nicer. However, please note that you will need to write the print statement explicitly if you work in normal Python (*.py*) scripts.

## Floating point numbers

Notice that while the previous addition of two integers results in another integer, the division results in a number with a decimal point. This is what we call a float (short for *floating point* number), which is a way computers represent real numbers.

All of the standard arithmetic operations that work on integers further also work on floats or for any combination of them:

```{code-cell}
roughly_pi = 3.14
radius = 2

print("The circumference of the cicle:", 2 * roughly_pi * radius)
```

## Strings

Strings are sequences of characters. In Python, we can define strings by encloding zero or more characters in a pair of quotes. It does not matter whether you use single or double quotes and both work equally well as long as the opening and closing quotes match.

```{code-cell}
module = "psy111"
university = 'Oldenburg'
```

There are many prebuilt functions you can use on strings, like figuring out their length:

```{code-cell}
len(module)
```

Or converting them to lower case:

```{code-cell}
university.lower()
```

We can even replace a substring with another substring:

```{code-cell}
university.replace("burg", "castle")
```

One thing you might have noticed is that these examples seem to use two different syntaxes. In the first example, `len()` seems to be a function which takes a string as its parameter (or *argument*), while in the other examples the function comes after the string with a *dot* notation `.upper()`. If this is a bit confusing do not worry, we will talk about this difference a bit later.

Another useful thing about string is that you can use *formatted* strings (f-strings) to nicely format strings when printing results. For this you can just add an *f* before the opening quotation marks of the string and you can then print the value of any variable by enclosing it with curly brackets `{}` in the middle of your string:

```{code-cell}
num_neurons = 86
print(f"The human brain has {num_neurons} billion neurons.")
```

You can also do many more things like formatting the number of decimal points shown for a number. See for example the Python documentation for more information: https://docs.python.org/3/tutorial/inputoutput.html.

## Booleans

Handling boolean values in Python is pretty much the same as in other programming languages. However, they can only take the value `True` (corresponding to 1) or `False` (corresonding to 0) and not other versions like `true` or `"False"`:

```{code-cell}
i_like_psy111 = True
i_like_psy111
```

One of the ways boolean values are typically generated in Python is through logical or comparison operations. For example, the statement "5 is larger than 3" can be answered in a binary way (it is either true or false):

```{code-cell}
5 > 3
```

Similarly, if we want to compare two numbers, this is also a logical operation that returns a Boolean value:

```{code-cell}
5 == 3
```

But what if the question you are trying to ask is not so simple? Python lets you built *conjunctions* of several subexpressions:

```{code-cell}
("burg" in "oldenburg") and (5 > 3) and (4 * 2 == 8)
```

In logical operations, *and* requires ALL statements to be true, wich is the case here. Alternatively, *or* requires only one of the statements to be true:

```{code-cell}
("burg" in "oldenburg") and (5 > 3) or (4 * 2 == 10)
```
This expression still returns `True` even though the last comparison is false due to it being joined with the previous expression through *or*. This brif example hopefully already illustrate nicely how the Python syntax is more readable than most other programming languages.

## None

So what if you want to create a variable but not assign a specific value to it? This is where *None* comes in handy. *None* works similar as for example the *NaN* (not a number) value from MATLAB. Please note that *None* and *False* however are not the same thing!

```{code-cell}
None == False
```

```{admonition} Summary
:class: tip

The Python standard library includes the following data types:

- Integer values (1, 2, 3)
- Floating point values (1.0, 2.2)
- Charater strings ("Hello", "World")
- Boolean values (True, False)
- No value (None)
```