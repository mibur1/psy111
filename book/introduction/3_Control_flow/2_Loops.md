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

# 3.2 Loops

Now, we will have a look at how we cann use looping operations to simplify repetition, allowing us to perform tasks across items or count through fixed numbers efficiently.

## Loops are repetition instructions

We can loop (or *iterate*) over elements in a collection. You can think about loops as a way of acting on each item one by one. In a `for` loop, we can read it as "for each item in a group". For example, if we want to read each page in a book, the following `for` loop acts means "for each page in the book, read it.":

```{code-cell}
pages = [1, 2, 3, 4, 5]

for page in pages:
    print(f"Reading page {page}")
```

Here, we loop over each item/element of the list starting from the first. In each iteration of the loop, we assign the value to a variable called `page` which is then printed. `page` is a temporary variable, which means it will only exist whithin the scope of the loop and not anymore after.

As a more practical application, imagine you’re in a classroom and need to hand out worksheets to each student. You wouldn’t want to say, "Hand this to the first student, then hand it to the second student, then hand it to the third student...":

```{code-cell}
students = ["Alice", "Bob", "Charlie", "Daisy"]

print(f"Handing worksheet to {students[0]}")
print(f"Handing worksheet to {students[1]}")
print(f"Handing worksheet to {students[2]}")
print(f"Handing worksheet to {students[3]}")
```

Instead, you’d say, “Hand this to every student one after the other.” This is the core of a loop: it lets you avoid doing the same task individually and instead repeats it for each item in a group.

```{code-cell}
students = ["Alice", "Bob", "Charlie", "Daisy"]

for student in students:
    print(f"Handing worksheet to {student}")
```

Now think about having 30 students, or even 100. Without a loop, you would then have to write the print statement 30 or 100 times, while the loop would stay completely identical and only take 2 lines of code. This is why using loops is such a useful thing to do!


## Counting loops: Doing something a fixed number of times

One very common use case of loops is to perform a specific action a set number of times. In Python, we can use the built-in `range()` function to count for us, so we don’t have to write each number ourselves. It’s like setting a timer for an exercise, where you know you’ll do an activity for 10 seconds or repeat it 5 times.

```{code-cell}
for i in range(5):
    print(f"Jumping jack #{i}")
```

Here, `range(5)` tells the loop to repeat 5 times. The variable `i` is called an index variable. Remember, `range()` starts at the first number and stops just before the last number, making the final value exclusive:

```{code-cell}
for i in range(5,10):
    print(f"Jumping jack #{i}")
```

Note that, as with the `len()` function, the end point of the `range()` function is exclusive, meaning it will create a sequence of numbers from 0 to 3. You can further also provide only a single number (like the length of a list) and use the numbers created by range as *indices* to index another variable:

```{code-cell}
my_list = ["apple", "banana", 3, 4]
list_length = len(my_list)

for i in range(list_length):
  print(my_list[i])
```

```{admonition} Question
:class: hint

Do you remember the differences between a tuple and a list? Why do you think does the `range()` function return a tuple instead of a list?
```

### Using enumerate()

You have previously seen two ways of looping over a list. Either by indexing the items directly, or through indexing by usinge the `range(len())` expression. Python also provides a nice way of combining these two through the `enumerate()` function:

```{code-cell}
for index, value in enumerate(my_list):
    print(f"Index {index} contains: {value}")
```

In cases where you need to keep track of the index, this approach is often preferred because it avoids manual indexing, making the code cleaner and less prone to errors.

## While loops: "Keep going until..."

`While` loops are like a task that repeats until a certain condition is met. For example, think of heating water in a kettle. You’ll keep heating the water until it reaches boiling point. In code, this looks like a `while` loop, which keeps running until a specified condition is no longer true.


```{code-cell}
temperature = 20  # starting temperature
while temperature < 100:
    print(f"Heating... temperature is {temperature}°C")
    temperature += 10  # increase temperature
print("Water is boiling!")
```

In this example, the loop keeps increasing the temperature until it reaches 100°C. Then, it stops. 

Be careful with `while` loops because if the condition never becomes `False`, the loop will run indefinitely, leading to an infinite loop.


## Using counters

Sometimes it can be useful to keep track to things happening in a loop. A concept you will see often is the use of counters:

```{code-cell}
counter = 0
while counter < 5:
    print(f"Counter is {counter}")
    counter += 1
```

Here, `counter` is a counting variable which is initialized and set to `0` before the loop. It then keep strack of something within a loop (in this case it makes sure the) and, importantly, lets you access this result later on in your script.

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

- For Loops: "For each" item in a group, do something.
- Range Loops: "Do this action X times" by counting up to a certain number.
- While Loops: "Keep doing this until..." a certain condition changes.

It is helpful to follow good coding practices:

- Avoid deep nesting. If you find your code nesting too deeply, consider refactoring parts of it into functions (which we will learn about next week).
- Use meaningful variable names. This helps others (and yourself) understand what the code is doing.
```

