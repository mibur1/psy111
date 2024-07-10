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

# 2.3 Collections

Variables containing only single values like an integer or a string will only get you so far. Many real-world applications for example require you to store a whole collection of values in a single data structure. Here, the most important and widely used solutions from the Python standard library are *lists*, *tuples*, and *dicts* (dictionaries).

## Lists

Lists are the most common collection in Python. They are a *heterogeneous* collection of objects, which means they are not limited to elements of a single type but can also multiple types if this is required. Lists are initalized through square brackets `[]` and its elements are separated through commas:

```{code-cell}
empty_list = []
random_stuff = ["apple", 3.14, True, 4]
```

### Indexing

Lists are ordered collections, which means that the order of items is important and will not change unless we specifically change it. This allows us to access individual elements of the list by specifying their position, which is also called *index*. The process of acessing individual or multiple values from a data structure is analogously called *indexing*.

To access the *i*th element in a list, we enclose the desired index *i* in square brackets. Note that Python, unlike for example MATLAB, uses zero-based indexing. This means the first element of a collection is at index 0, while index 1 returns the second element (and so forth).

```{code-cell}
random_stuff[0]
```

```{code-cell}
random_stuff[1]
```

### Slicing

What if you do not just want to retrieve more than a single element from a list? For this we can use *slicing*, which uses the colon (:) operator:

```{code-cell}
random_stuff[1:3]
```

The colon is used to seperate a starting and a stopping index. Intuitively you can read this notation as "from random_stuff get items 1 to 3". Note that the starting position is *inclusive* (meaning it includes the item at position 1, which in this case is *3.14*) while the stopping index is *exclusive* (meaning that the item at posititon 3, which here would be *4*, is not included).

### Modifying lists

Lists are *mutable* objects, which means they can be modified after they have been created. For example, we can replace a specific element with another element:

```{code-cell}
print("Before re-assignment:", random_stuff)
random_stuff[0] = "banana"
print("After re-assignment:", random_stuff)
```

Another common use case is to add new values to a list (for example once new results have been calculated). This can be done by using the `.append()` function on the list:

```{code-cell}
random_stuff.append("goodbye")
random_stuff
```

## Tuples

Tuples are similar to lists in that they are an ordered colletion of elements. However, they are *immutable* in nature, meaning you can not change their content after creating them. For creating tuples, you simply use the round brackets instead of the square ones:

```{code-cell}
my_tuple = ("Hello", 1, 2, 3, 4, 2, "Goodbye")
```

Tuples only have two built-in methods which are `.count()` and `.index()`:

```{code-cell}
first_occurrence = my_tuple.index(2)
print(f"The first occurrence of 2 is at index: {first_occurrence}.")

count_of_twos = my_tuple.count(2)
print(f"The number 2 appears {count_of_twos} times in the tuple.")
```

In case you still need a mutable version of the tuple, you can convert any tuple to a list by using the `list()` function:

```{code-cell}
converted_from_tuple = list(my_tuple)
```

## Dictionaries

Dictionaries (*dicts*) are another popular data structure. In short, dicts are mappings from keys to values. You can think of them as key/value pairs, where keys within a single dictionary need to be unique, while the values do not. Most programming languages have similar structures, for example [in MATLAB](https://de.mathworks.com/help/matlab/ref/containers.map.html) they are called *maps*.

Dictionaries are created by using curly brackets, and can either be initialized as empty of with key/value pairs separated by commas.

```{code-cell}
empty_dict = {}

example_dict = {
  "name": "Alice",
  "age": 26,
  1: "integer_key",
  (1, 2): "tuple_key",
  "list_value:": [0.5, 0.3]
}
```

As you can see, dicts are quite versatile. Keys are required to be *immutable* like a string, number, or tuple (this is so they do not unexpectedly change during your program and make things really messy). The values, however, can be of any type you want!

Accessing values stored in a dictionary is slightly different. While you previously accessed items in lists by their index, values in a dictionary are not ordered and have to be accessed by their key. The syntax is identical to that used for list indexing, we specify the key as a string between square brackets:

```{code-cell}
example_dict["name"]
```

Dictionaries can be updated or, if the key does not exist yet, extended through the same []-based syntax, except you now have to make an assignement using the (=) operator:

```{code-cell}
example_dict["age"] = 27
```


```{admonition} Summary
:class: tip

The three main collection types in Python are:

- Lists `my_list = ["a","b","c"]`
- Tuples `my_tuple = (1,2,3)`
- Dictionaries `my_dict = {"course": "psy111"}`
```