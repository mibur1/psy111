---
kernelspec:
  name: python3
  display_name: Python 3
---

# 4.3 Functions

Functions are essential in programming as they allow you to encapsulate and reuse code efficiently. By breaking down complex problems into smaller, manageable parts, functions enhance code organization and readability. As we already saw you can use built-in functions or import them from specialized modules. Quite often it is also useful for you to *define* your own functions. In Python, this can be done by using the `def` keyword followed by a function name:

```{code-cell} ipython3
def my_function():
    print("I'm a custom function!")

my_function() # use the function
```

## Function arguments and returns

Functions can accept inputs that modify their behaviour. Strictly speaking, the names listed in the function definition are called *parameters*, while the actual values you pass when calling the function are called *arguments* (in practice, the two terms are often used interchangeably). For example, if we want to add two numbers, we can pass both of them as arguments and *return* the result to the caller:

```{code-cell} ipython3
def add(a, b):
    return a + b

result = add(1, 2)
print(result)
```

### Required and optional parameters

Function parameters can either be *required* or *optional*. Required parameters do not have a default value and *must* be provided, otherwise the function will throw an error. In the previous example, both `a` and `b` are required parameters, and the function would not know what to do if any of them were missing. Optional parameters on the other hand are assigned a *default* value in the function definition. This means that the function knows what to do even if the user does not explicitly provide a value for that parameter.

As an example, let's create a function that checks whether a reaction time is valid. This is a common step when cleaning data from psychological experiments, as you might want to exclude trials in which participants responded unrealistically fast (e.g. anticipations) or very slowly (e.g. lapses of attention). Only the reaction time `rt` is required, while the lower and upper limits of the valid range are optional:

```{code-cell} ipython3
def is_valid_rt(rt, min_rt=150, max_rt=1000):
    """Checks whether a reaction time lies within a valid range.

    Parameters
    ----------
    rt : float
        The reaction time in milliseconds.
    min_rt : float, optional
        The fastest reaction time that is still considered valid.
        Default: 150
    max_rt : float, optional
        The slowest reaction time that is still considered valid.
        Default: 1000

    Returns
    -------
    bool
        True if the reaction time lies within the valid range,
        False otherwise.
    """
    return min_rt <= rt <= max_rt
```

```{note} Docstrings

So far, you have mostly seen comments within the code using the `#` sign. The text enclosed in triple double quotes `"""..."""` at the beginning of the `is_valid_rt()` function is a so-called *docstring*. Unlike comments, docstrings are stored together with the function, so you (or anyone else using your code) can later look them up with the `help()` function. In the `is_valid_rt()` function, the input and output parameters are clearly described, making it easier to understand how to use the function. Writing docstrings is the official way of documenting functions and classes in Python, and the format used here (the [NumPy style](https://numpydoc.readthedocs.io/en/latest/format.html)) is one of the most common conventions.
```

```{code-cell} ipython3
help(is_valid_rt)
```

If we now call this function by just providing a reaction time, it will still work as expected by using the default range of 150 to 1000 ms:

```{code-cell} ipython3
is_valid_rt(450)
```

```{code-cell} ipython3
is_valid_rt(1200)
```

### Positional and keyword arguments

When calling a function, there are two ways of passing arguments. *Positional* arguments are matched to the parameters by their position, while *keyword* arguments are matched by their name (e.g. `max_rt=1500`). If your task allows for slower responses, you can simply pass a new upper limit as a keyword argument. As keyword arguments are identified by their name, you can provide them in any order you like, as long as all positional arguments come before them.

```{code-cell} ipython3
is_valid_rt(1200, max_rt=1500)
```

You can also pass all arguments (including `rt`) by name, in which case their order does not matter at all

```{code-cell} ipython3
is_valid_rt(max_rt=1500, min_rt=200, rt=1200)
```

or pass all of them by position:

```{code-cell} ipython3
is_valid_rt(1200, 1500, 200)
```

However, the last example gives a different result than the previous one! As no names are provided, Python matches the arguments to the parameters in the order in which they are defined in the function, so they are interpreted as `rt=1200`, `min_rt=1500` and `max_rt=200`. With these limits, no reaction time could ever be valid. Keyword arguments are therefore a good way of making your code both clearer and less error-prone.

### Variable numbers of arguments

Another feature of Python is the ability to write functions that accept an arbitrary number of arguments by using `*args` and `**kwargs`. While we will not cover this topic here, I want to mention it for completeness. If you are interested or encounter it in practice, feel free to explore this topic on your own (e.g., [here](https://book.pythontips.com/en/latest/args_and_kwargs.html)).

```{tip} Summary

- Functions are defined with the `def` keyword and can `return` a result.
- Parameters without a default value are required, parameters with a default value are optional.
- Arguments can be passed by position (`is_valid_rt(1200, 200, 1500)`) or by name (`is_valid_rt(1200, max_rt=1500)`).
- Docstrings (`"""..."""`) document what a function does and can be displayed with `help()`.
```
