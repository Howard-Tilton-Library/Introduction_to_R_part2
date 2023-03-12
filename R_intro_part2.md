---
title: "Intro to Coding in R, Part II: Data Management and Processing"
author: "Mike Ellis (he/him/his) <br> PhD Candidate <br> Ecology & Evolutionary Biology <br> Tulane University <br> mellis5@tulane.edu"
date: "12 March 2023"

format: 
  html:
    toc: true
    toc-location: left
    toc-depth: 2
    number-sections: true
    number-depth: 1
    theme: lumen
    highlight-style: github
    code-overflow: wrap
    code-fold: false
    code-copy: true
    code-link: false
    code-tools: false
    code-block-border-left: "#0C3823"
    code-block-bg: "#eeeeee"
    fig-cap-location: margin
    linestretch: 1.25
    fontsize: "large"
    embed-resources: true

execute:
  echo: true
  keep-md: true
---



<br>

Welcome! This tutorial was commissioned by Tulane University’s Howard-Tilton Memorial Library as the second half of an introduction to R workshop. 
It is targeted towards faculty, post-docs, graduate students, and undergraduates new to R. 
In part I, you learned the fundamentals of R and built a foundation for working with your own data. In this second installment, you'll learn to streamline project management, incorporate packages to increase functionality, code more efficiently in the "tidy" dialect, implement more complex conditional and iterative processes, summarize and analyze your data, and visualize your results with figures.

Happy coding!

<br>

# **Start a New Project**

- What is a project
- Why are they convenient
- How to make one
- Version control (brief)
- Folder organization
- File/folder paths

# **Working with Packages**

- Packages pane
- Base R packages

## Installing

- Install button
- Versions and updating

## Loading

- one or more at a time
- Where to put it in your script
- calling a single function with `::` (useful esp. with overlapping names)

# **Enter the Tidyverse**

- What is it
- Vs. "base" R
- Component packages
- https://www.tidyverse.org/learn/
- Pipelines
- Cheatsheet(s) - from RSTudio? Source?
- Useful functions

# **Grouping & Summarizing Data**

## `group_by()`

- example with count or something

## `summarize()`

- n(), mean(), sd(), min(), max()

# **If/Else Statements **

# **For Loops**

# **T-Tests**

# **Figures in R**

- r-graphs-gallery
- ggplot
- Alternative to boxplots
- Bar chart
- Saving/Exporting