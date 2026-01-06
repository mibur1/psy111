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

# CFA & SEM

**Confirmatory Factor Analysis (CFA)** and **Structural Equation Modelling (SEM)** are used to test *theory-driven hypotheses* about latent variables and their relationships to observed data. In contrast to Exploratory Factor Analysis (EFA), where the factor structure is discovered from the data, CFA and SEM require the researcher to specify the model in advance:

* how many latent variables exist,
* which observed variables measure them, and
* how the latent variables are related.

This makes CFA and SEM particularly well suited for testing psychological theories and measurement models.

---

## From CFA to SEM

You can think of CFA as a special case of SEM.

* In CFA, the focus lies on the *measurement model*: observed variables load on latent factors, and latent factors may correlate or form higher-order factors.
* In SEM, the model is extended by adding *directional (regression) paths* between variables—typically between latent constructs.

In short:

> CFA tests how constructs are measured; SEM tests how constructs are related.

---

## Typical Steps in CFA/SEM

1. **Specify the model**  
   Based on theory, decide how many latent variables exist and which observed variables indicate each latent variable.

2. **Identify (scale) latent variables**  
   Because latent variables have no natural scale, they must be identified (e.g. by fixing one loading or fixing the latent variance).

3. **Estimate the model**  
   Fit the specified model to the data using an SEM software package.

4. **Evaluate model fit**  
   Assess how well the model reproduces the observed data using fit indices and, if necessary, compare alternative models.

---

## Conceptual Example

Assume we measure:

* Working Memory (WM) using five items, and
* General Processing Speed (GPS) using three items.

If each set of items loads on its respective latent factor, and the two factors are allowed to correlate, this is a CFA.

If we additionally assume that both *WM and GPS reflect a broader construct* such as Intelligence (I), we can model I as a higher-order factor, which is still CFA.

However, if we instead hypothesise that *WM predicts GPS*, and specify a directional path from WM to GPS, the model becomes a SEM, because it includes structural (regression) relationships between latent variables.
