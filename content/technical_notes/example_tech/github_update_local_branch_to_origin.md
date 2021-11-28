---
date: "2021-11-28T00:00:00+01:00"
draft: false
linktitle: Update local branch to origin
menu:
  example_tech:
    parent: Git & GitHub
    weight: 1
title: Update local branch to upstream origin
toc: true
type: docs
weight: 1
---

## Update local branch to upstream origin

**Situation**: The branch you're working off has fallen behind from the origin, you need to get it up-to-date. *This* allow you to get your local branch up-to-date with 'origin' **without** having to switch over the `develop` branch to run `git pull`, you can do it right in the current branch. 

This is the command to run in terminal. Make sure you're in the appropriate project directory. 

```{python}
# ensure we are working off the latest version of the upstream repo
git pull origin

# bring the latest changes into our local branch
git merge develop

# resolve conflicts...

# push the latest version of the local branch...
```

See this [stack overflow](https://stackoverflow.com/questions/7773939/show-git-ahead-and-behind-info-for-all-branches-including-remotes/20499690#20499690) for a command to see ahead/behind for all branches.




For more content on data science, R, and Python [find me on Twitter](https://twitter.com/paulapivat).






