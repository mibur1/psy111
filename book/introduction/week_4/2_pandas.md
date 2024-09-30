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

# 4.1 Pandas

Many kinds of real-world data are stored in a tabular format. This means two-dimensional tables structured into rows and columns, with each observation typically taking up a row and each column representing a single variable.


The Pandas library is a popular Python library for dealing with tabular data. When importing any library you can chose an abbreviation for it. In principle, this can be anything you want (or none at all), however it usually makes sense to stick to the conventions:

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

The variable `yeatman_data` now is a Pandas DataFrame which contains our data and we can use the `.head()` method to look at the first few rows of the data. The leftmost column `subjectID` is the index column, while the other columns contain the data for different variables such as age, gender, or IQ. You can also see that the DataFrame is *heterogeneously typed* meaning it can contain variables of different types (e.g. strings or floats). You can also already see some missing values. For example, `subject003` and `subject_004` are missing values for IQ related columns.

### Summarizing DataFrames

Pandas further contains useful methods to e.g. summarize the data. We can use `.info()` to get a closer look into our data:

```{code-cell}
print(yeatman_data.info())
```

Most of this information should already make sense. An observation that can be made is that Gender and Handedness are stored as objects. This is because they are a mixture of not only strings but also NaNs, which are considered numerical values.


We can also get a first statistical summary for the numerical columns by using the `.describe()` method. NaN values are ignored for these calculations, but the `count` column will tell you how many values were used for the calculations in each column.

```{code-cell}
print(yeatman_data.describe())
```

### Indexing DataFrames

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

If we assign this column to a new variable, it will result in a Pandas `Series`, which is a one dimensional series of values. Series are pretty similar to DataFrames (essentially DataFrames are just a collection of Series). However, as they contain only one variable, we do not need `.loc` or `.iloc` (though they still work ), but we can directly index by the subject ID or the row:

```{code-cell}
age = yeatman_data["Age"]
print(age['subject_072'])
print(age[72])
```

Series are useful as we can for example create a new *subset* of a DataFrame. A new DataFrame containing only the variables `Age` and `IQ` can be created by indexing it with a list of columns:

```{code-cell}
yeatman_subset = yeatman_data[["Age", "IQ"]]
print(yeatman_subset)
```


