---
title: "diffViz"
excerpt: "Highlight and visualize differences<br/><img src='/images/diffViz-demo.png'>"
collection: portfolio
---


To highlight the differences in an easy-to-understand manner, diffViz visually separates the common and different blocks of a pair of pre-processed traces (or any pair of text-based files) by implementing the [*diff* algorithm](https://link.springer.com/article/10.1007/BF01840446).
*diff* takes two sequences A and B and computes the minimal edit to convert A to B. This algorithm is used in the [GNU diff](https://www.gnu.org/software/diffutils/) utility to compare two text files and in [git](https://git-scm.com/docs/git-diff) for efficiently keeping track of file changes.
diffViz aligns common and different blocks of a pair of sequences (e.g., traces) horizontally and vertically, making it easier for the analyst to see the differences at a glance.

![alt text]( https://staheri.github.io/images/diffViz-demo.png "DiffViz Demo")

diffViz is now available from [this Github repository](https://github.com/staheri/diffViz).
