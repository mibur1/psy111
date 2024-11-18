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

# 3.1 Conditionals

Conditional statements (if-else statements) are essential for writing programs that can make decisions based on varying inputs and conditions. They help in implementing logic that responds differently depending on the circumstances.

## Conditionals as "if-then" decisions

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

 The `else` statement is what we call a *fallback* option, which means it will run only if none of the previous conditions are met. Both *elif* (which is short for *else if*) and *else* statements are optional; you could simply use *if* statements to handle every possible scenario. However, this can lead to worse code readability, especially if many decisions are possible. Now, regardless of the weather, you’re covered with either an umbrella or sunglasses.

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

## Decisions with multiple conditions

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

There are times when you want to act if either of two conditions is true. For instance, if you’ll go out if it’s either sunny or warm, you can use `or` in the conditional:

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

