---
date: "2021-11-24T00:00:00+01:00"
draft: false
linktitle: Fix Linting Error
menu:
  example_tech:
    parent: Webdev
    weight: 2
title: Fixing linting errors
toc: true
type: docs
weight: 2
---

## Fixing Linting Errors

**Situation**: You want to push up a Pull Request, but if any files has **linting errors** it'll be problematic with the PR. 

## Indicators of Linting Errors

When you open `terminal` and see the "Problem" tab, linting errors will be show here. You can also see `red` lines on any file itself. Sometimes your machine expects "tabs", but you used "spaces". 

The command to fix linting error for a particular file (change directory into the problematic file):

```{python}
$ yarn linting file_name.js --fix
```

For more content on data science, R, and Python [find me on Twitter](https://twitter.com/paulapivat).