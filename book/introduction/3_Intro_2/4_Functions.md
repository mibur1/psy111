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

# 3.4 Functions

Functions are essential in programming as they allow you to encapsulate and reuse code efficiently. By breaking down complex problems into smaller, manageable parts, functions enhance code organization and readability. As we already saw you can use built-in functions or import them from specialized modules. Quite often it is also useful for you to *define* your own functions. In Python, this can be done by using the `def` keyword followed by a function name:

```{code-cell}
def my_function():
  print("I'm a custom function!")

my_function() # use the function
```

## Function arguments and returns

Functions can accept *arguments* (or parameters) that provide inputs or modify their behaviour. For example, if we want to add two numbers, we can pass both of them as arguments and further *return* the result to the user:

```{code-cell}
def add(a, b):
  return a + b

result = add(1, 2)
print(result)
```

### Arguments

Python functions can have two different kinds of arguments: *positional* arguments, and *keyword* arguments. Positional arguments on the one hand are defined by their position whithin the round brackets and *must* be provided for the function to run without giving an error. In the previous example, both `a` and `b` are positional arguments and the function would not know what to do if any of them would be missing. Keyword arguments on the other hand are assigned a *default* value. This means that the function knows what to do even if the user does not explicitly provide a value for that argument. As an example, lets create a function that adds random gaussian noise to a given input `x:

```{code-cell}
import random

def add_noise(x, mu=0, sd=1):
  """Adds gaussian noise to the input.

  Parameters
  ----------
  x : number
    The number to add noise to.
  mu : float, optional
    The mean of the gaussian noise distribution.
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

*Note:* So far you mostly saw comments whithin the code by using the # sign. Another way of commenting your code is to use three double parentheses `"""comment"""` as shown in the previous code snippet. This is useful if you need to provide larger portions of text like when documenting your functions. You can see one example of that in the `add_noise()` function, which clearly describes the input and output parameters of the function, making it easier to understand how to use the function.

If we now call this function by just providing a value for `x`it will still work as expected by using a mean of 0 and a standard deviation of 1 to calculate and add the noise.

```{code-cell}
add_noise(5)
```

If you decide you need different noise with a standard deviation of 5, you can simply add this new value. As positional arguments are optional, their order does not matter. You can provide any keyword argument in any order you like, as long as you provide its name and all positional arguments have been correctly provided before.

```{code-cell}
add_noise(5, sd=5)
```

You can further use positional arguments by their name

```{code-cell}
add_noise(x=5, sd=5)
```

or without any name at all

```{code-cell}
add_noise(5, 5)
```

However, the last example will produce a different result than the other ones! This is because if no keywords are provided, Python will only be able to rely on the order of arguments, which would result in the arguments being interpreted as `x=5`, `mu=5` and `sd=1` as per default.

### Argument unpacking

Another feature of Python is providing a function with an unspecified number of arguments by using `*args` and `**kwargs`. We will not cover this here, but please feel free to explore this topic on your own (e.g. [here](https://book.pythontips.com/en/latest/args_and_kwargs.html)).