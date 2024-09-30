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

# 4.2 Numpy

Before we begin, let's pause for a second and try to think about which kind of data types and data structures structures we have learned about so far. Which ones can you remember? Do you think this is already enough for any kind of task you might want to achieve?

```{admonition} Solution
:class: dropdown

Python includes well known data types such as:
- Integer numbers: 1,2,3, ...
- Floating point numbers: 1.2, 3.14, ...
- Strings: "Hello"
- Boolean values: True/False
- *Empty* value: None

The most important built-in data strucutures are:
- Lists: [item1, item2, ...]
- Tuples: (item1, item2, ...)
- Dictionaries: {"key": value(s)}
```

## Arrays

In fields like neuroscience data sets can be large and can often have more than two dimensions. For example, think of an fMRI scan composed of individual voxels (cubes) in three-dimensional space. And naturally, if you obtained more than one fMRI scan over time, you might have this as a fourth dimension. In such cases, **n-dimensional arrays** are a good way of storing and handling your data, as they do so as a single object. In any case, arrays are *contiguious*, meaning they have no "holes" and *homogenous*, meaning they only consist of a single data type.

We can simply import the numpy library at the top of the script. In principle, you can chose any abbreviation you want (or none at all), however it usually makes sense to stick to the conventions.

```{code-cell}
import numpy as np
```

We can then go on and create an array as follows:

```{code-cell}
my_aray = np.array([1,2,3],
                   [4,5,6])
print(my_array)
print(my_array.shape)
print(my_array.dtype)
```

This previous example created a 2-dimensional array (you could also call it a matrix) of shape 2x3. An advantge of using is that we can then use *vectorized* operations to access and deal with data whithin them. This is much more efficient than writing naive loops to iterate over the array.