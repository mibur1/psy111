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

# Multilevel Models

Multilevel models, also called linear mixed models (LMMs), are a statistical approach to simultaneously model predictors at different levels of data. These models help account for variability within and between nested groups, such as individuals within clusters or organizations. Multilevel regression is particularly useful for analyzing nested sources of variability, allowing researchers to explore questions about relationships both within and between these levels.


## A Practical Example

Imagine we have data from patients in three different hospitals, and we aim to predict "will to live" (Y) based on "symptom severity" (X). At first glance, the data may suggest a positive relationship between "will to live" and "symptom severity": the more severe the symptoms, the higher the will to live. This finding might appear counterintuitive.

```{figure} figures/multilevel_reg1.jpg
---
name: multilevel_reg1
---
Screenshot taken from [Quant Psych (Youtube)](https://www.youtube.com/watch?v=5tOifM51ZOk)
```

When we color-code the data points by hospital, however, a different pattern emerges. Within each hospital, the relationship between "will to live" and "symptom severity" is negative—  as symptom severity increases, the will to live decreases, which is a more intuitive result.

```{figure} figures/multilevel_reg2.jpg
---
name: multilevel_reg2
---
Screenshot taken from [Quant Psych (Youtube)](https://www.youtube.com/watch?v=5tOifM51ZOk)
```

If we ignore the clustering variable "hospital," we risk being misled by the data. This is because data points within a single hospital tend to influence one another. For example:

- **Blue Hospital**: This hospital might exclusively treat severe cases (all blue data points score high on the X variable). However, it could have highly skilled psychologists who boost their patients' will to live (high scores on Y).
- **Red Hospital**: This hospital might primarily admit mild cases (low scores on X) but provide poor-quality care, leading to generally low scores on Y.

In this scenario, the variable "staff quality" could explain the differences in "will to live" (Y) between hospitals. Here, hospitals are level-2 units, and patients are level-1 units. Variables like "staff quality" that explain differences between level-2 units are called level-2 variables.

Even within clusters, there is variability. For instance:

- Some patients with less severe symptoms report a very low will to live.
- Others with more severe symptoms report a relatively high will to live.

This variability within hospitals is at level 1 and could be explained by factors like individual "resilience." Variables that explain differences between individuals (level-1 units) are referred to as level-1 variables.

```{admonition} Summary
:class: note

- Multilevel models account for nested data structures, such as patients nested within hospitals.
- Ignoring clustering variables can lead to misleading conclusions, as relationships may differ within and between clusters.
- These models allow researchers to distinguish between level-1 (within-cluster) and level-2 (between-cluster) variability, providing deeper insights into the data.
```