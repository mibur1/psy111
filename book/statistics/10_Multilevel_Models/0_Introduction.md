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

Multilevel regression (also called linear mixed models (LMMs)) resemble an approach to simultaneously model predictors at different data levels. 
For data with nested sources of variability (e.g., individuals in classes within schools), multilevel regression can be used to address 
research questions related to each level of the data. 
For example, within- and between-person relationships or between-person and between-schools associations can be studied simultaneously.

**Think of the following example.**

Imagine we have data from patients from three different hospitals and
we want to predict will to live (Y) with symptom severity (X).
If we look at the data we might say that there is a positive relationship
between will to live (Y) and symptom severity (X), meaning the
more severe ones symptoms are the more someone ones to live (that sounds
a bit suspicious, doesn’t it?).

```{figure} figures/multilevel_reg1.jpg
---
width: 60%
name: multilevel_reg1
---
Adapted from [Quant Psych (Youtube)](https://www.youtube.com/watch?v=5tOifM51ZOk)
```

However, if we color-code the data points to see from which
of the three hospitals we see that within each cluster the relationship
is (as you might expect) negative, meaning with
increasing symptom severity the will to live decreases.

```{figure} figures/multilevel_reg2.jpg
---
width: 60%
name: multilevel_reg2
---
Adapted from [Quant Psych (Youtube)](https://www.youtube.com/watch?v=5tOifM51ZOk)
```

If we would not include the clustering variable ’hospital’ we’d be misled by
the data because we didn’t account for the fact that the data points within
one hospital influence each other. An explanation for this pattern might
be that the blue hospital only receives patients with severe symptoms (all
blue data points score high on the X variable) but also has top-notch psychologists
that increase the patients will to live (high scores on Y variable).
The red hospital on the other hand only receives mild case (low scores on
X variable) but treats its patients badly which results in a general lower
will to live (low scores on Y variable). This shows that ’staff quality’ might
be an explanation for the difference in Y between the hospitals. In this example
the hospital are the level-2 units and the patients level-1 units.
The variable ’staff quality’ (maybe) explains the differences on Y between
hospitals (level-2 units) and is therefore a level-2 variable. Also, we see that
even within clusters there’s some variability: Some patients have less severe
symptoms but a really low will to live, while others have worse symptoms
but a relatively high will to live. This is variability on level-1 that might be
explained by (for example) the ’resilience’ of the individual patients. Since
patients are level-1 units, variables that explain differences between them
(as for example varying ’resilience’) are called level-1 variables.