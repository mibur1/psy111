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

# 2.4 Control flow

In this chapter, we will explore the fundamentals of control flow in Python. Control flow allows you to dictate the order in which statements are executed, enabling your programs to make decisions and perform repetitive actions.

## Conditionals

Conditional statements (if-else statements) are essential for writing programs that can make decisions based on varying inputs and conditions. They help in implementing logic that responds differently depending on the circumstances.

```{code-cell}
number = 5

if number < 5:
  print("Number is smaller than 5.")
elif number == 5:
  print("Number is equal to 5.")
else:
  print("Number is larger than 5.")
```

Depending on the value of `number`, only the `print()` statement that matches the condition will be evaluated. The `else` statement acts as a *fallback* option, which means it will run only if none of the previous conditions are met. Both *elif* (which is short for *else if*) and *else* statements are optional; you could simply use *if* statements to handle every possible scenario. However, this can lead to worse code readability, especially if many decisions are possible.

## Loops

Loops allow us to *iterate* (or loop) over elements of a collection. Let's for example consider a list of numbers:

```{code-cell}
list_of_numbers = [2, 4, 6, 8, 10]

for number in list_of_numbers:
  print(number)
```

Here, we loop over each item/element of the list starting from the first. In each iteration of the loop, we assign the value to a variable called `number` which is then printed. `number` is a temporary variable, which means it will only exist whithin the scope of the loop and not anymore after.

### Looping over a range

A very common use case of loops is to perform a specific action *n* times. For example, if you have 30 participants in your study, you want to iterate over all of them to perform some action. For this, you can use the built-in `range()` function to provide a range of numbers:

```{code-cell}
print("Range 1:")
for r in range(4):
  print(r)

print("Range 2:")
for r in range(1,10,2):
  print(r)

```

Note that, as with the `len()` function, the end point of the `range()` function is exclusive, meaning it will create a sequence of numbers from 1 to 3. You can further also provide only a single number (like the length of a list) and use the numbers created by range as *indices* to index another variable:

```{code-cell}
my_list = ["apple", "banana", 3, 4]
list_length = len(my_list)

for i in range(list_length):
  print(my_list[i])
```

### Using enumerate()

You have previously seen two ways of looping over a list. Either by indexing the items directly, or through indexing by usinge the `range(len())` expression. Python also provides a nice way of combining these two through the `enumerate()` function:

```{code-cell}
for index, value in enumerate(my_list):
    print(f"Index {index} contains: {value}")
```

In cases where you need to keep track of the index, this approach is often preferred because it avoids manual indexing, making the code cleaner and less prone to errors.

### While loops

In addition to `for` loops, you can also use `while` loops, which continue to run as long as a specified condition is `True`:

```{code-cell}
counter = 0
while counter < 5:
    print(f"Counter is {counter}")
    counter += 1
```

Be careful with `while` loops because if the condition never becomes `False`, the loop will run indefinitely, leading to an infinite loop.

The preious code cell also introduces a counter variable, which is a useful thing to keep in mind when you need to keep track of something within a loop and access this result later on in your script.

## Nested statements

In more complex analyses, you often need to nest multiple statements inside one another. If you for example want to iterate over an entire list and perform an action only for specific items of the list you could to this as follows:

```{code-cell}
my_list = ["apple", "banana", 3, 4]

for item in my_list:
  if item == "banana":
    print("Banana!")
  else:
    print("Not a banana.")
```

## Whitespace is syntactically required

One important thing which we have not yet covered explicitly is how the code whithin if-statements or for-loops is indented (shifted to the right). You might think this is just a way of making the code easier to read, and that would be true for almost all other programming languages. However, Python is a bit different in that regard by **requiring** you to use whitespace with certain rules.

Simply put, whenever you use a *compound statement* (which includes for-loops, conditionals, and also classes and functions which we will cover later), you need to increase the indentation of your code. Once you exit the compound statement, you decrease the indentation level by the same amount. The amound of this indentation is technically up to you, however the Python style guide recommends you to use four spaces. As an example, if we want to continue with the script after the for-loop, we would reset the indentation to the same level as the first for:

```{code-cell}
for item in my_list:
  if item == "banana":
    print("Banana!")

print("Continue after the for-loop...")
```

```{admonition} Summary
:class: tip

Python includes standard control flow concepts:

- Conditional statements (`if`, `elif`, `else`) allow programs to make decisions.
- Loops (`for` and `while`) enable repetitive tasks.
- Proper indentation is required in Python, otherwise you will get an error.

It is helpful to follow good coding practices:

- Avoid deep nesting. If you find your code nesting too deeply, consider refactoring parts of it into functions (which we will learn about next week).
- Use meaningful variable names. This helps others (and yourself) understand what the code is doing.
```

