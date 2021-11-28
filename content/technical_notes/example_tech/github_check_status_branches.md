---
date: "2021-11-28T00:00:00+01:00"
draft: false
linktitle: Show ahead/behind all branches
menu:
  example_tech:
    parent: Git & GitHub
    weight: 1
title: Show ahead/behind for all local branches
toc: true
type: docs
weight: 1
---

## Show ahead/behind for all local branches

**Situation**: You can do `git status` to see how far ahead or behind the *current* branch your work is, but sometimes you want to see the status of **all** branches. 

This is the command to run in terminal. Make sure you're in the appropriate project directory. 

```{python}
$ git for-each-ref --format="%(refname:short) %(upstream:track) %(upstream:remotename)" refs/heads

```

See this [stack overflow](https://stackoverflow.com/questions/7773939/show-git-ahead-and-behind-info-for-all-branches-including-remotes/20499690#20499690).




For more content on data science, R, and Python [find me on Twitter](https://twitter.com/paulapivat).






