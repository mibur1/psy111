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

# 3.5 Classes
test

As previously mentioned, [everything in Python is an object](1_Everything_is_an_object). The key cocept behind this is the object-oriented programming (OOP) paradigm central to many programming languages. I will try to briefly these concepts in this chapter, however, do not worry if you feel like these concepts do not immediately make sense to you. Learning a programming language is like learning a real language and takes time. I am however convinced that once the semester goes on many of these newly introduced concepts will already start to click as you work your way through the exercises.

## So what is a class?

A class is, in a sense, a kind of template for an object. You can think of it as a set of rules and instructions what an object of a given kind can do. Let us start with the simplest example of a class and built on top of that.

```{code-cell}
class Circle:
  pass
```

These two lines of code already define a perfectly valid class called `Circle` (note that it starts with a capital C, which is a naming convention for classes). The `pass` statement is used as a placeholder to tell the Python interpreter that it should not expect any more code to follow for the class.

## Creating instances

So what can we do with the Circle class? It doesn't particularly look useful, does it? However, we can already do the most important thing we can do with a class, we can create a new *instance* of said class, which is to say we create objects whose behavior is defined by the `Circle` class. You can kind of imagine this as drawing different circles on a piece of paper. All of them are circles, but none of them would be the actual definition of a circle.

The syntax for creating an instance in Python is simple:

```{code-cell}
my_circle = Circle()
```

The `my_circle` variable contains an object, which is a specific instance of the `Circle` class. As of now, this object does neither contain any important information nor can it do anything.

## Making it do things

Let's start by providing some features to the `Circle` class:

```{code-cell}
from math import pi

class Circle:
  def __init__(self, radius):
    self.radius = radius

  def area(self):
    return pi * self.radius**2
```

We now defined two *methods* (functions whithin a class). First, there is the `__init()__` method, which might look fairly strange to you. This is a so called *magic method*, which is the basic initialization that is used every time we create an instance of our class. The `__init()__` method has two arguments. The first parameter is called `self`. It is a mandatory argument for any method whithin a class to take a reference to the current instance (*itself* so to say). The second argument `radius` then the first actual argument which has to be provided if a circle object is created. Whithin the `__init()__` method, this argument is assigned to a variable called `self.radius` which makes it an *attribute* of the class instance.

The `area()` method simply defines a way of calculating the area of the circle. As a method of the class it has access to all class attributes (variables starting with `self.`) and thus does not need any additional parameters other than the mandatory `self`.

We can then use this newly updated Circle class as follows:

```{code-cell}
my_circle = Circle(4)
my_circle.area()
```

The first line creates an object of the Circle class with radius 4. We can then use the `.area()` method of that object to calculate its area. Pretty cool, huh? If we change the radius of the given circle, then the area will change as well:

```{code-cell}
my_circle.radius = 9
my_circle.area()
```

## Magic methods

We came across the concept of magic methods like the `__init()__` method that runs every time we create an instance of an object. There are many other methods like this, and the key concept already is that they are usually called implicitly when a certain operation is applied to an object, even though it does not look like the operation and the magic method hav anything to do with each other. That's what makes them magic!🪄

You are probably already tired of me saying "everything in Python is an object". However, you will now see one of the deeper implications this has. *All operators in Python are just cleverly disguised method calls*. That means even if we write something seemingly basic like `4 * 3`, Python will implicitly call a magic method on the first operant (the number 4 as an object of the integer class), with the second operand being passed as an argument. This might sound a bit confusing, but

```{code-cell}
4 * 3
```

under the hood is essentially just

```{code-cell}
(4).__mul__(3)
```

with the parentheses being necessary so the `.` is not confused to be a decimal point. This also works for strings:

```{code-cell}
"hello" + " world"
```

```{code-cell}
"hello".__add__(" world")
```