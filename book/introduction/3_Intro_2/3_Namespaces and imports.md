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

# 3.3 Namespaces and imports

Python is a high-level, dynamic programming language often associated with flexibility (for example, you do  not need to explicitly declare the type of your variables), but there are other cases in which Python is very strict (for example when it comes to the whitespace). Another strict thing about Python is that it takes *namespaces* very seriously.

If you are used to other programming languages like MATLAB or R, you might expect to have hundreds of functions available to call as soon as you start your script. By contrast, Python only offers a very small built-in namespace, which means the number of functions you can use is very small. This is done on purpose, you are expected to carefull manage the namespace and functions you use.

If you require any additional functions, you will need to *import* it from whatever module it is in via the `import` statement. While this might seem strange at first, you will see that it significantly improves readability and code clarity compared to for example MATLAB. As an example, you might want to use the square root for some kind of calculations. The corresponding function would be `sqrt()` in the `math` module:

```{code-cell}
import math

result = math.sqrt(9)
print(f"The quare root of 9 is {result}")
```

The previous code cell imports the entire math module, which means you will also have access to all its other functions (e.g. the sine function `math.sin()`). You can also import just specific functions/submodules or rename them:

```{code-cell}
import numpy as np    # import the numpy module as np
from math import sqrt # import the sqrt() function

print(np.sqrt(9), sqrt(9))
```

In this example, we use a square root function from two different packages. First, from the numpy library which we call `np` (so we don't have to write numpy all the time), and second, directly from the math library so we can omit the `math.` prefix in the following code. As a rule of thumb, you should import an entire module if you use many of its functions and only a single function if you do not need anything else from that module. This approach helps in keeping the namespace clean and improves code readability.