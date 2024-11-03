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

# 2.6 More control flow

## More on conditionals

In this section, we will briefly recap how conditionals make your code responsive to different scenarios, from simple "if-then" choices to more complex decisions.

### Conditionals as "if-then" decisions

Conditionals work like everyday decisions. Imagine you’re deciding what to wear based on the weather. You check if it’s raining, and if it is, you wear a raincoat. This is the essence of an `if` statement: it allows a program to choose an action based on whether a condition is true or false:

```{code-cell}
weather = "rainy"

if weather == "rainy":
    print("Take an umbrella!")
```

Here, we’re saying: *“If the weather is rainy, then take an umbrella.”* If it’s not rainy, this code does nothing, and no message is printed.

However, conditionals often need a backup plan if the initial condition isn’t met. Continuing the weather example, what if it’s not raining? You might want to say, *“If it’s not raining, wear sunglasses.”* This is where `else` comes in:

```{code-cell}
weather = "sunny"

if weather == "rainy":
    print("Take an umbrella!")
else:
    print("Wear sunglasses!")
```

Now, regardless of the weather, you’re covered with either an umbrella or sunglasses.

Real decisions often involve more than two choices. Let’s say you want to decide what to wear based on three types of weather: rain, snow, or sun. You could use an `if` for the first option, `elif` (short for “else if”) for the second, and an `else` for anything else (so this is always your *fallback*, covering anything not included in your other statements):

```{code-cell}
weather = "snowy"

if weather == "rainy":
    print("Take an umbrella!")
elif weather == "snowy":
    print("Wear a warm coat!")
else:
    print("Wear sunglasses!")
```

### Decisions with multiple conditions

Sometimes, a decision depends on multiple factors. For example, if you’re deciding to go for a run, you might only go if the weather is sunny and you have enough time. This uses both conditions combined with `and`:

```{code-cell}
weather = "sunny"
time = "enough"

if weather == "sunny" and time == "enough":
    print("Go for a run!")
else:
    print("Maybe stay home.")
```

Now, both `weather == "sunny"` and `time == "enough"` must be true to go for a run. If either one is false, the code will suggest staying home.

There are times when you want to act if either of two conditions is true. For instance, if you’ll go out if it’s either sunny or warm, you can use or in the conditional:

```{code-cell}
weather = "cloudy"
temperature = "warm"

if weather == "sunny" or temperature == "warm":
    print("Let's go outside!")
else:
    print("Stay inside.")
```

Now, as long as it’s sunny or warm, you’ll go outside. If neither condition is true, you’ll stay inside.

```{admonition} Summary
:class: tip

- `if`: "If this condition is true, do something."
- `else`: "Otherwise, do something else."
- `elif`: "If the first condition isn’t true, check another condition."
- `and` / `or`: Combine multiple conditions for more complex decisions.
```


## More on loops

Now, we will have another look at how loops simplify repetition, allowing you to perform tasks across items or count through fixed numbers efficiently.

### Loops are repetition instructions

Imagine you’re in a classroom and need to hand out worksheets to each student. You wouldn’t want to say, "Hand this to the first student, then hand it to the second student, then hand it to the third student...":

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


### Loops and "for each" language

Another way to think about loops is as a way to act on each item one by one. In a `for` loop, we can read it as "for each" item in a group. For example, if we want to read each page in a book, the `for` loop acts like, "for each page in the book, read it.":

```{code-cell}
pages = [1, 2, 3, 4, 5]

for page in pages:
    print(f"Reading page {page}")
```

This loop goes page by page without us having to write each page number manually.

### Counting loops: Doing something a fixed number of times

Sometimes, we just want to do something a set number of times. In Python, we can use range() to count for us, so we don’t have to write each number ourselves. It’s like setting a timer for an exercise, where you know you’ll do an activity for 10 seconds or repeat it 5 times.

```{code-cell}
for i in range(5):
    print(f"Jumping jack #{i}")
```

Here, `range(5)` tells the loop to repeat 5 times. The variable `i` is called an index variable. Remember, `range()` starts at the first number and stops just before the last number, making the final value exclusive:

```{code-cell}
for i in range(5,10):
    print(f"Jumping jack #{i}")
```

```{admonition} Question
:class: hint

Do you remember the differences between a tuple and a list? Why do you think does the `range()` function return a tuple instead of a list?
```

### While loops: "Keep going until..."

`While` loops are like a task that repeats until a certain condition is met. For example, think of heating water in a kettle. You’ll keep heating the water until it reaches boiling point. In code, this looks like a `while` loop, which keeps running until a specified condition is no longer true.


```{code-cell}
temperature = 20  # starting temperature
while temperature < 100:
    print(f"Heating... temperature is {temperature}°C")
    temperature += 10  # increase temperature
print("Water is boiling!")
```

In this example, the loop keeps increasing the temperature until it reaches 100°C. Then, it stops.


```{admonition} Summary
:class: tip

- For Loops: "For each" item in a group, do something.
- Range Loops: "Do this action X times" by counting up to a certain number.
- While Loops: "Keep doing this until..." a certain condition changes.
```
