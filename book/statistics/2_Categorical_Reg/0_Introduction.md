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

# Categorical Regression

In our previous seminar, we covered Generalized Linear Models (GLMs), partial correlation, and their implementation in Python. Today, we will incorporate categorical variables, such as male/female or patients versus controls. As discussed in the lecture, selecting an appropriate coding scheme is essential for addressing our research questions. We will focus on the following four coding schemes from the lecture:

- Dummy coding
- Unweighted effects coding
- Weighted effects coding
- Contrast coding

After familiarizing yourself with these coding schemes, you will apply your new skills to a dataset related to heart disease.

## Example data set

**Backround**: Alzheimer’s Disease (AD) is a progressive disorder characterized by the degeneration and death of brain cells. One of the most extensively studied *genetic risk factors* for AD is a variant of the apolipoprotein E gene (APOE). Specifically, the APOE e4 allele has been associated with an increased risk of developing Alzheimer's Disease.

**Research Aim**: predict figural working memory ability (WMf) based on genotype groups.