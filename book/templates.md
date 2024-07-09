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

# Micellaneous templates for MyST syntax

Inline directive to refer to a document: {doc}`overview`.

Citing: {cite}`Rokem2023`.

Code cells with a directive like so:

```{code-cell}
print(2 + 2)
```

Boxes. Possible classes are:
- blue: note, important
- green: hint, seealso, tip
- yellow: attention, caution, warning
- red: danger, error

```{admonition} Summary
:class: tip

Hello world
```


