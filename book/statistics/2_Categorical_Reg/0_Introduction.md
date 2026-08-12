
# 🏷️ Categorical Regression

In our previous seminar, we covered Generalized Linear Models (GLMs), partial correlation, and their implementation in Python. Today, we will incorporate categorical variables, such as male/female or patients/controls. As discussed in the lecture, selecting an appropriate coding scheme is essential for addressing these research questions. We will focus on the following four coding schemes from the lecture:

- Dummy coding
- Unweighted effects coding
- Weighted effects coding
- Contrast coding

After familiarizing yourself with these coding schemes, you will apply your newly learned skills to a dataset related to heart disease.

## The Dataset

**Backround**: Alzheimer’s disease (AD) is a progressive disorder characterized by the degeneration and death of brain cells. One of the most extensively studied *genetic risk factors* for AD is a variant of the apolipoprotein E gene (APOE). Specifically, the APOE e4 allele has been associated with an increased risk of developing Alzheimer's disease.

**Research Aim**: Predict figural working memory ability (WMf) based on genotype groups.