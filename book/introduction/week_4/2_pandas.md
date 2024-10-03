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

# 4.2 Pandas

Many kinds of real-world data are stored in a tabular format. This means two-dimensional tables structured into rows and columns, with each observation typically taking up a row and each column representing a single variable.

The Pandas library is a popular Python library for dealing with tabular data. In comparison to numpy, pandas specifically limits us to two-dimensional tables, but we gain the flexibility of e.g. having variables of different types.

```{code-cell}
import pandas as pd
```

## DataFrames

DataFrames are the core data structure in the Pandas library. They are ideal for working with tabular data, making it easy to manipulate, filter, and analyze datasets. We can create and print a simple example as follows:

```{code-cell}
my_df = pd.DataFrame({'A': ['A0', 'A1', 'A2', 'A3'],
                      'B': ['B0', 'B1', 'B2', 'B3'],
                      'C': ['C0', 'C1', 'C2', 'C3']})
print(my_df)
```

We can also read data from other sources such as Excel or CSV (comma-separated values) files. CSV files are a good way of storing tabular data as they are plain text, meaning you can open and read them with any kind of text editor or programming language. Usually, CSV files have a header row containing the labels (column names).

As an example, let us load some data that was collected as part of a study of life span changes in brain tissue properties {cite}`Yeatman2014`. We can simply load this file from the internet by using the `pd.read_csv()` function. The only argument required for this is the data we want to load. In this case, we provide an URL as the data is stored on the internet, however, you would usually provide a path pointing to a file on your computer. All other arguments are optional, but they can be useful in providing additional instructions for what to do with the data. Here, we use `usecols=[1,2,3,4,5,6,7]` to specify only some specific columns, `na_values="NaN"` to specify that missing values should be entered as NaN ("not a number"), and `index_col=0` to use the first column as an our index:

```{code-cell}
yeatman_data = pd.read_csv("https://yeatmanlab.github.io/AFQBrowser-demo/data/subjects.csv",
                      usecols=[1,2,3,4,5,6,7],
                      na_values="NaN",
                      index_col=0)
print(yeatman_data.head())
```

The variable `yeatman_data` now is a Pandas DataFrame which contains our data and we can use the `.head()` method to look at the first few rows of the data. The leftmost column `subjectID` is the index column, while the other columns contain the data for different variables such as age, gender, or IQ. You can also see that the DataFrame is, in contrary to the previously introduced numpy arrays, *heterogeneously typed*. This means it can contain variables of different types (e.g. strings or floats). You can also already see some missing values. For example, `subject003` and `subject_004` are missing values for IQ related columns.

### Summarizing

Pandas further contains useful methods to e.g. summarize the data. We can use `.info()` to get a closer look into our data:

```{code-cell}
print(yeatman_data.info())
```

Most of this information should already make sense. An observation that can be made is that Gender and Handedness are stored as objects. This is because they are a mixture of not only strings but also NaNs, which are considered numerical values.


We can also get a first statistical summary for the numerical columns by using the `.describe()` method. NaN values are ignored for these calculations, but the `count` column will tell you how many values were used for the calculations in each column.

```{code-cell}
print(yeatman_data.describe())
```

### Indexing

In previous sessions we already leearned about indexing and slicing as a way of accessing individual elements of e.g. lists. Pandas DataFrames also support a variety of different indexing and slicing operations. For example, we can select rows through the `.loc` attribute and by indexing in square brackets:

```{code-cell}
print(yeatman_data.loc["subject_000"])
```

In the case that we do not know the exact name of the subject but just its index (e.g. we cant to access the first subject), we can use the `iloc` attribute for that purpose:

```{code-cell}
print(yeatman_data.iloc[0])
```

This returns the same information, as instead of looking for `subject_000` we are asking for the first row at position 0. You now might ask yourself how we can index a two-dimensional table with just one index. The answer is that it is just a shorthand form of the full expression:

```{code-cell}
print(yeatman_data.iloc[0, :])
```

Remember from previous sections that `:` stands for "all values". This means we can also apply slicing to extract a subset of columns:

```{code-cell}
print(yeatman_data.iloc[0, 2:5])
```

Similarly, we can also access a single column:

```{code-cell}
print(yeatman_data.iloc[:, 0])
```

However, while `.loc` and `.iloc` are powerful attibutes, we can also simply address columns directly by their name:

```{code-cell}
print(yeatman_data["Age"])
```

If we assign this column to a new variable, it will result in a Pandas `Series`, which is a one dimensional series of values. Series are pretty similar to DataFrames (essentially DataFrames are just a collection of Series):

```{code-cell}
age = yeatman_data["Age"]
print(age['subject_072'])
print(age.iloc[72])
```

Series are useful as we can, for example, create a new *subset* of a DataFrame containing only the variables `Age` and `IQ` can be created by indexing it with a list of columns:

```{code-cell}
yeatman_subset = yeatman_data[["Age", "IQ"]]
print(yeatman_subset.head())
```

### Computations

Like NumPy arrays, Pandas DataFrames also have many methods that allow for computations. However, as we only deal with tabular data, the dimensions are always the same, with the columns being the variables and the rows being the observations. One can simply calculate the means of all the variables in the DataFrame:

```{code-cell}
yeatman_means = yeatman_data.mean(numeric_only=True)
print(yeatman_means)
print(yeatman_means["Age"])
```

Since not all variables are numeric, we include a `numeric_only=True)` as an argument of the mean function. We can also directly calculate the mean for individual series:

```{code-cell}
yeatman_data["IQ"].mean()
```

We can further perform arithmetics on DataFrames. For example, we could calculate a standardized z-score for the age of each subject.

```{code-cell}
age_mean = yeatman_data["Age"].mean()
age_std = yeatman_data["Age"].std()
print((yeatman_data["Age"] - age_mean ) / age_std)
```

A useful thing is to then save the result as a new variable in our DataFrame. For example, we can create a new column called `Age_std` and assign our results to it:

```{code-cell}
yeatman_data["Age_std"] = (yeatman_data["Age"] - age_mean ) / age_std
print(yeatman_data.head())
```

### Filtering

Similar to logical indexing in NumPy, we can also filter our data set based on some properties. For example, let's assume we only want be able to filter subjects below the age of 18 in our analysis. We can then simply create a new boolean variable in the DataFrame which codes for this condition:

```{code-cell}
yeatman_data["Age_below_18"] = yeatman_data["Age"] < 18
print(yeatman_data.head())
```

As you can see, we have now extended our original DataFrame by another column which tells us if the correspoding subjects are

### MultiIndex

Sometimes we want to select groups made up of combinations of variables. For example, we might want to analyze the data based on both gender and age. One way of doing this is to change the index of the DataFrame to be made up of more than one column. This is called a *MultiIndex DataFrame*. We can do so by applying the `set_index()` method of a DataFrame to create a new kind of index:

```{code-cell}
multi_index = yeatman_data.set_index(["Gender", "Age_below_18"])
print(multi_index.head())
```

You can now see that we have two indices. This means we can apply the `.loc` method to select rows based on both indices:

```{code-cell}
male_below_18 = multi_index.loc["Male", True]
print(male_below_10.describe)
```

This might already seem useful, but it can become quite cumbersome if you want to repeat this for many kind of combinations. And because grouping data into different subgroups is such a common pattern in data analysis, Pandas offers a built-in way of doing so, which we will explore in the following subsection.

### Split-Apply-Combine

A usual problem we are faced with in data analysis is the following: We (1) want to take a data set and split it into subsets, (2) we then independently apply some operation to each subset and (3) combine the results of all independent operations into a new data set. This pattern ins called *split-apply-combine*.

For example, let's start with splitting the data by the `Gender` column:

```{code-cell}
gender_groups = yeatman_data.groupby("Gender)
```

The newly `gender_grous` variable is a `DataFrameGroupBy` object, which is pretty similar to a normal DataFrame, with the additional feature of having distinct groups whithin. This means we can perform many operations just as if we would be working with a normal DataFrame, with the only difference being the operation being applied independently to each subset.

For example, we can calculate the mean for each group:

```{code-cell}
print(gender_groups.mean())
```

The output of this operation is a DataFrame that contains the summary with th original DataFrame's `Gender` variable as the index. This means we can apply standard indexing operations on it as well to get e.g. the mean age of female subjects:

```{code-cell}
print(gender_groups.mean().loc["Female", "Age"])
```

We can further group by multiple indices:

```{code-cell}
gender_age_groups = yeatman_data.groupby(["Gender, "Age_below_18"])
print(gender_age_groups.mean())
```

### Joining Tables

Another useful feature of Pandas is its ability to join data. For example, lets assume we have three DataFrames with the same columns but different indices. This could for example happen if you would measure the same variables for multiple subjects over three different measurement days. So the index would be the individual subject, and the three DataFrames would be the data you aquired on e.g. Monday, Tuesday, and Wednesday:

```{code-cell}
df1 = pd.DataFrame({'A': ['A0', 'A1', 'A2', 'A3'],
                    'B': ['B0', 'B1', 'B2', 'B3'],
                    'C': ['C0', 'C1', 'C2', 'C3'],
                    'D': ['D0', 'D1', 'D2', 'D3']},
                    index=[0, 1, 2, 3])
df2 = pd.DataFrame({'A': ['A4', 'A5', 'A6', 'A7'],
                    'B': ['B4', 'B5', 'B6', 'B7'],
                    'C': ['C4', 'C5', 'C6', 'C7'],
                    'D': ['D4', 'D5', 'D6', 'D7']},
                    index=[4, 5, 6, 7])
df3 = pd.DataFrame({'A': ['A8', 'A9', 'A10', 'A11'],
                    'B': ['B8', 'B9', 'B10', 'B11'],
                    'C': ['C8', 'C9', 'C10', 'C11'],
                    'D': ['D8', 'D9', 'D10', 'D11']},
                    index=[8, 9, 10, 11])
```

Here it might be intuitive to just concatenate them into one big DataFrame:

```{code-cell}
combined_df = pd.concat([df1, df2, df3])
print(combined_df)
```

In this case, we see that the concatenation is quite straightforward and succesful. But what about if the DataFrames are not of identical structure? Let's assume we have `df4` which has index values $2$ and $3$ as well as columns `B`and `D`in common with `df1`, but it also has the additional indices $6$ and $7$ ad well as a new column `F`:

```{code-cell}
df4 = pd.DataFrame({'B': ['B2', 'B3', 'B6', 'B7'],
                    'D': ['D2', 'D4', 'D6', 'D7'],
                    'F': ['F2', 'F3', 'F6', 'F7']},
                    index=[2, 3, 6, 7])

combined_df = pd.concat([df1, df4])
print(combined_df)
```

The results look a bit more interesting. Here, Pandas tried to preserve as much information as possible, meaning the new DataFrame contains all columns/variables present in the two original DataFrames. Wherever possible, Pandas will merge the columns into one. This is true for column `D`, as it exists in both original DataFrames. For all other columns, Pandas preserves the input values and adds `NaN`s for the missing values.

There are also other, more complicated, scenarios which we will not talk about here. For example, you might want to concatenate along the second axis instead of the first one. Don't be afraid of trying things out if you are ever in need of something more detailed. Getting used to working with data sets takes time, but no matter your specific goal, it will more likely than not be possible with just a few lines of code.

## Errors

Before closing this section, I would like to emphazize on a few patterns of errors that are unique to Pandas and which you most likely will encounter at some point in your own projects.

One common pattern of errors comes from a confusion between Series and DataFame objects. And while we previously learned that they are indeed pretty similar, they still have some differences. For example, Series objects have a useful `.value_counts()` method that creates a table with the number of observations in the Series for every unique value. DataFrames however do not implement this method and will rause a Python `AttributeError`instead.

Another common error comes from the fact that many operations create a new DataFrame as an output insted of changing the current one in place. For example you might expect that:

```{code-cell}
yeatman_data.dropna()
```

will remove the `NaN` values from the DataFrame. However, it will not do so on the `yeatman_data` DataFrame itself but you need to assign it to a new variable if you want to keep this result:

```{code-cell}
yeatman_without_nan = yeatman_data.dropna()
```

or alternatively, **explicitly** specify that the existing `yeatman` DataFrame should be modified:

```{code-cell}
yeatman_data.dropna(inplace=True)
```

These kind of errors are especially dangerous, as you could unknowingly continue working with an unchanged DataFrame, leading to erroneous results later on in your script. It therefore makes sense to at least periodically check the intermediate results in your calculations to spot errors early.

Finally, indexing errors are also common. Don't be discouraged by such errors, as indexing can indeed be confusing in the beginning, especially with different ways of doing so such as indexing in NumPy, indexing by rows and columns, and indexing with `.loc` or `.iloc`.