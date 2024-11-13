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

# 3.1 Everything is an Object

So far, and analogous to many other programming languages, you might think of data types like strings or integers as *primitive* data types, which means that they are built into the core of the language to behave in special ways and to not be changed or modified in any way. And this is true in many
other programming languages like C++. where you will have to explicitly tell the program what type of variable you want to create.

In Python this is a bit different and there aren't *really* any primitive datata types, as it is a deeply *object-oriented* programming language. In Python, *everything is an object*. Strings are objects, integers are objects, lists are objects, dictionairies are objects, and so fourth. We will come back to that concept in detail later today, but for now let us just focus on some practical implications this will bring.


## The dot notation

Let's start with the dot (.) notation. As you probably already noticed, we previously worked with our variables in two different ways. First, there is a functional syntax, where we pass an object (e.g. a list) as an argument to a function:

```{code-cell}
len([4,3,2,1])
```

A function is a specific, reusable piece of code, that performs a specific action. In this case, the `len()` function calculates the length of the list given to it as an argument. It then returns some kind of result, which we can print or save as a new variable. Second, there is the object-oriented syntax which you previously saw when working with strings:

```{code-cell}
text = "Hello World"
text.lower()
```

If you have previously only worked with data-centric languages like MATLAB you might find this a bit confusing. But don't worry, it essentially means that we simply call a function attached to the object (in this case the string). Such object specfic functions are called *methods*, so if you use `text.lower()` it will simply call the `.lower()` method of the string object. The dot operator expresses a relationship of belonging, so you can intuitively read `text.lower()` as "on the text variable, use the .lower() method which belongs to it".


### Inspecting objects

One implication of everything being an object in Python is that we might need to find out what kind of data an object contains and which methods it implements, as this might not always be obvious from just looking at the variable itself. We will not go into too much detail, but briefly introduce some simple ways for you to interrogate objects.

First, you can always see the type of an object by using the built-in `type()` function:

```{code-cell}
my_list = [1, 2, 3]
type(my_list)
```

Second, the `dir()` function will show you all methods implemented by an object as well as their *static attributes*, which are variables stored whithin the object (they often start and end with two underscores).

```{code-cell}
dir(my_list)
```

Here you can see, that the list object implements 11 different methods starting with `append`. All of these attributes and methods are available to you to access through the dot notation (e.g. `my_list.__class__` or `my_list.append()`).

```{code-cell}
my_list.__class__
```

```{code-cell}
my_list.append(4)
my_list
```

```{admonition} Summary
:class: tip

Everything in Python is an object!

- Functions like `len()` are standalone blocks of code that can be called independently of objects.
- Methods like `.lower()` are functions that belong to objects.
- You can inspect the type and content of objects with the `type()` and `dir()` functions.
```