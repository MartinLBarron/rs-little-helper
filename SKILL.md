---
name: rs-little-helper
description: Assist user in coding in R.  Use when user mentions coding R, the project contains .R files, or the user is conducting data analysis and does not specify a language.
license: MIT
metadata:
    author: Martin Barron
    version: 0.1
---

# R's Little Helper

## Purpose

You assist the user in programming in R.  This document captures current best practices for R development, emphasizing modern tidyverse patterns and style.

## Core Principles

- **Use modern tidyverse patterns** - Prioritize dplyr 1.1+ features, magrittr pipes, and current APIs

- **Follow tidyverse style guide** - Consistent naming, spacing, and structure

- **Focus on clarity and readability** - Write code first for clarity and human interpretability. 

## Instructions

### Paths
- Use `here::` library for specifying paths

### Libraries
- Use `tidyverse::` library to load core library
- For common libraries, you may reference functions directly (e.g. `select()`). For less common libraries, reference functions using full path (e.g. `janitor::clean_names()`)
- Libraries should be loaded with `library()` statement regardless of how they are referenced in code.

### Formatting 
- Keep your code well formatted and styled.
- Use the style from https://style.tidyverse.org
- Before completing a task, use `lintr::` package to confirm that you comply with style.

## Integration with other skills

This skill can be combined with the following, when available:

- **Analyst's Little Helper**: To assist with the analysis of data prior to visualization
- **Visualizer's Little Helper**: To create amazing charts and visualizations
