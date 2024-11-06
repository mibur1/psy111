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

# 8.1 Dataset
The data we'll be working with today may look familiar, as we've already used it in previous sessions. However, our focus will now shift to a new research question that is specifically suited for moderated regression analysis.

**Research Question**: Investigate whether the association between fluid intelligence (`gff`) and figural working memory (`WMf`) is moderated by age (`age`).

Before we begin the regression analysis, we'll perform basic descriptive analyses and visualize the variables.

Here, we’ll try a new approach by first subsetting the variables into a smaller data frame, although this step isn’t strictly necessary for the analysis.

```{code-cell}
import pandas as pd

# Load the dataset and subset into smaller dataframe
df = pd.read_csv("data/data.txt", delimiter='\t')

dataframe=df[['age', 'subject', 'WMf', 'gff']]

```
## Descriptive Analysis

We'll start by examining the first few rows of our new subset using 'head()' and then apply 'describe()' to get an overview of key statistics, such as the mean and standard deviation.

```{code-cell}
print(dataframe.head())
dataframe.describe()
```
## Descriptive Plots
To further visualize the data, we can use Seaborn's histplot to examine the frequency distribution of the age variable. This will give us a clearer view of how age is distributed across our sample.

```{code-cell}
#import libraries
import matplotlib.pyplot as plt
import seaborn as sns

#Histogram for Age
sns.histplot(df['age'], bins=9)  # kde=True adds a density curve
plt.xlabel('Age')
plt.ylabel('Frequency')
plt.title('Age Distribution')
plt.show()
```
By using Seaborn's 'scatterplot', we can explore the relationship between 'WMf' and 'gff' and identify any trends. Additionally, we will incorporate age as the hue to visualize how this variable may influence the relationship between 'WMf' and 'gff'.

```{code-cell}
sns.scatterplot(data=dataframe, x='WMf', y='gff', hue='age')
plt.xlabel('WMf')  # Label for x-axis
plt.ylabel('gff')  # Label for y-axis
plt.title('Scatter Plot of WMf vs. gff, colored by Age')  # Title for the scatter plot
plt.show()
```



