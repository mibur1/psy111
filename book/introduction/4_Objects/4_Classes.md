---
kernelspec:
  name: python3
  display_name: Python 3
---

# 4.4 Classes

As previously mentioned, [everything in Python is an object](1_Everything_is_an_object). The key concept behind this is the object-oriented programming (OOP) paradigm central to many programming languages. You will get a brief introduction to these concepts in this chapter, however, do not worry if you feel like these concepts do not immediately make sense to you. Learning a programming language is like learning a real language and takes time. I am however convinced that once the semester goes on many of these newly introduced concepts will already start to click as you work your way through the exercises.

## So what is a class?

A class is, in a sense, a kind of template for an object. You can think of it as a set of rules and instructions for what an object of a given kind can do. Let us start with the simplest example of a class and build on top of that.

```{code-cell} ipython3
class Circle:
    pass
```

These two lines of code already define a perfectly valid class called `Circle` (note that it starts with a capital C, which is a naming convention for classes). The `pass` statement is a placeholder that does nothing. It is needed because the body of a class (just like that of a function, loop, or if-statement) cannot be empty.

## Creating instances

So what can we do with the Circle class? It doesn't particularly look useful, does it? However, we can already do the most important thing we can do with a class, we can create a new *instance* of said class, which is to say we create objects whose behavior is defined by the `Circle` class. You can kind of imagine this as drawing different circles on a piece of paper. All of them are circles, but none of them would be the actual definition of a circle.

The syntax for creating an instance in Python is simple:

```{code-cell} ipython3
my_circle = Circle()
```

The `my_circle` variable contains an object, which is a specific instance of the `Circle` class. As of now, this object neither contains any useful information nor can it do anything.

## Making it do things

Let's start by providing some features to the `Circle` class:

```{code-cell} ipython3
from math import pi

class Circle:
    def __init__(self, radius):
        self.radius = radius

    def area(self):
        return pi * self.radius**2
```

We now defined two *methods* (functions within a class). First, there is the `__init__()` method, which might look fairly strange to you. This is a so called *magic method*, which is the basic initialization that is used every time we create an instance of our class. The `__init__()` method has two parameters. The first one is called `self` and is a reference to the current instance (the object *itself*, so to say). Every method within a class needs it as its first parameter. Importantly, you never pass `self` yourself, as Python does that for you automatically whenever you call a method. The second parameter, `radius`, is therefore the only argument that has to be provided when a circle object is created (e.g. `Circle(4)`). Within the `__init__()` method, this argument is assigned to a variable called `self.radius`, which makes it an *attribute* of the instance.

The `area()` method simply defines a way of calculating the area of the circle. As a method of the class it has access to all attributes of the instance (variables starting with `self.`) and thus does not need any additional parameters other than `self`.

We can then use this newly updated Circle class as follows:

```{code-cell} ipython3
my_circle = Circle(4)
my_circle.area()
```

The first line creates an object of the Circle class with radius 4. We can then use the `.area()` method of that object to calculate its area. Pretty cool, huh? If we change the radius of the given circle, then the area will change as well:

```{code-cell} ipython3
my_circle.radius = 9
my_circle.area()
```

## Magic methods

We came across the concept of magic methods like the `__init__()` method that runs every time we create an instance of an object. There are many other methods like this, and the key idea is that they are usually called implicitly when a certain operation is applied to an object, even though it does not look like the operation and the magic method have anything to do with each other. That's what makes them magic! 🪄

You are probably already tired of me saying "everything in Python is an object". However, you will now see one of the deeper implications this has. *All operators in Python are just cleverly disguised method calls*. That means even if we write something seemingly basic like `4 * 3`, Python will implicitly call a magic method on the first operand (the number 4 as an object of the integer class), with the second operand being passed as an argument. This might sound a bit confusing, but

```{code-cell} ipython3
4 * 3
```

under the hood is essentially just

```{code-cell} ipython3
(4).__mul__(3)
```

with the parentheses being necessary so the `.` is not confused with a decimal point. This also works for strings:

```{code-cell} ipython3
"hello" + " world"
```

```{code-cell} ipython3
"hello".__add__(" world")
```

```{tip} Summary

- A class is a template for creating objects (*instances*), e.g. `my_circle = Circle(4)`.
- The `__init__()` method runs every time a new instance is created and is typically used to set its attributes.
- Methods are functions defined within a class. Their first parameter `self` refers to the instance itself and is passed automatically.
- Magic methods like `__init__()` or `__add__()` are called implicitly by Python, for example when using operators like `+`.
```