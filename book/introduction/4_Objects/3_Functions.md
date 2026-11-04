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

As an example, let's create a function that adds random Gaussian noise to a given input `x`:

```{code-cell} ipython3
import random

def add_noise(x, mu=0, sd=1):
    """Adds Gaussian noise to the input.

    Parameters
    ----------
    x : number
        The number to add noise to.
    mu : float, optional
        The mean of the Gaussian noise distribution.
        Default: 0
    sd : float, optional
        The standard deviation of the noise distribution.
        Default: 1

    Returns
    -------
    float
    """

    noise = random.normalvariate(mu, sd)
    return x + noise
```

```{note} Docstrings

So far, you have mostly seen comments within the code using the `#` sign. The text enclosed in triple double quotes `"""..."""` at the beginning of the `add_noise()` function is a so-called *docstring*. Unlike comments, docstrings are stored together with the function, so you (or anyone else using your code) can later look them up with the `help()` function. In the `add_noise()` function, the input and output parameters are clearly described, making it easier to understand how to use the function. Writing docstrings is the official way of documenting functions and classes in Python, and the format used here (the [NumPy style](https://numpydoc.readthedocs.io/en/latest/format.html)) is one of the most common conventions.
```

```{code-cell} ipython3
help(add_noise)
```

If we now call this function by just providing a value for `x` it will still work as expected by using a mean of 0 and a standard deviation of 1 to calculate and add the noise.

```{code-cell} ipython3
add_noise(5)
```

### Positional and keyword arguments

When calling a function, there are two ways of passing arguments. *Positional* arguments are matched to the parameters by their position, while *keyword* arguments are matched by their name (e.g. `sd=3`). If you decide you need different noise with a standard deviation of 3, you can simply pass this new value as a keyword argument. As keyword arguments are identified by their name, you can provide them in any order you like, as long as all positional arguments come before them.

```{code-cell} ipython3
add_noise(5, sd=3)
```

You can also pass all arguments (including `x`) by name, in which case their order does not matter at all

```{code-cell} ipython3
add_noise(x=5, sd=3, mu=2)
```

or pass all of them by position:

```{code-cell} ipython3
add_noise(5, 3, 2)
```

However, the last example is different compared to the previous one! As no names are provided, Python matches the arguments to the parameters in the order in which they are defined in the function, so they are interpreted as `x=5`, `mu=3` and `sd=2`.

### Variable numbers of arguments

Another feature of Python is the ability to write functions that accept an arbitrary number of arguments by using `*args` and `**kwargs`. While we will not cover this topic here, I want to mention it for completeness. If you are interested or encounter it in practice, feel free to explore this topic on your own (e.g., [here](https://book.pythontips.com/en/latest/args_and_kwargs.html)).

```{tip} Summary

- Functions are defined with the `def` keyword and can `return` a result.
- Parameters without a default value are required, parameters with a default value are optional.
- Arguments can be passed by position (`add_noise(5, 0, 3)`) or by name (`add_noise(5, sd=3)`).
- Docstrings (`"""..."""`) document what a function does and can be displayed with `help()`.
```
