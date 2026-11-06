<div style="padding-top:1em; padding-bottom: 0.5em;">
<img src="logo.png" width =200 align="right" />
</div>

# Multivariate Statistics with Python

## Online usage (recommended)

Course materials for the psy111 seminar of the Neurocognitive Psychology Master's course at the University of Oldenburg. The content should primarily be accessed from the online book: [![Jupyter Book Badge](https://raw.githubusercontent.com/jupyter-book/jupyter-book/next/docs/media/images/badge.svg)](https://michaburkhardt.de/psy111/)

## Exercises 
The *.ipynb* notebooks for the exercises can either be opened locally or through Google Colab by clicking the download button at the top.

## Local usage

Install the dependencies once:

```bash
cd <path/to/psy111>
pip install -r requirements.txt
```

Rendering the path diagrams in the SEM chapters also needs the Graphviz system package (`sudo apt install graphviz`, `brew install graphviz`, or `conda install graphviz`).

Then start a live preview server, which prints a link (usually <http://localhost:3000>):

```bash
jupyter book start --execute
```

The `--execute` flag runs the code cells. Without it the preview builds much faster but all outputs stay empty, which is often what you want while editing text.

To produce the static site that gets deployed to GitHub Pages:

```bash
jupyter book build --html --execute
```
