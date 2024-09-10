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

# Correaltion
By using the seaborn or pandas package, we will be able to use corr(). The beforehand defined dataframe is then used to calculate the correlations

you can also use scipy.stats.pearsonr to have it like cor.test() like in R with the p-value

here is an example
real python website

# Correation Matrix
when trying to compute more than one correlation between variables, it gets very messy without a better way of visualization. For that isntance one uses 

Problem described by kaplan 2009:

 golf tends to be played by richer people; and it is also known
 that on average the number of children goes down with increasing income. In other
 words, we have a fairly strong negative correlation between playing golf and the
 number of children, and one could be tempted to (falsely) draw the conclusion that
 playing golf reduces the fertility. But in reality it is the higher income which causes
 both effects.
Impliment first a simple matrix and then the heatmap

