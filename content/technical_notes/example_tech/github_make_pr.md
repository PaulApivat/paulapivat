---
date: "2020-11-24T00:00:00+01:00"
draft: false
linktitle: Making a Pull Request
menu:
  example_tech:
    parent: GitHub
    weight: 1
title: Making a Pull Request on a GitHub Repo
toc: true
type: docs
weight: 1
---

## Quick Guide for contributing to another code base by making a pull request

1. Identify a project you want to contribute to on GitHub
2. Fork that project
3. Clone it to your local machine*

**note** : Do not clone in another directory that was *already* cloned from another repo. For example, when I cloned [pytalentsolution](https://github.com/PaulApivat/pytalentsolution) on my local machine, I initially cloned it *inside* Saku directory, which was *itself* a clone of an already-existing-repo. The correct way was to create a *new* directory for pytalentsolution.

4. Make a new branch

- 4a: `cd` into directory of the cloned repo
- 4b: use `git branch` to confirm you're on the `*master` branch
- 4c: use `git checkout -b name_of_new_branch` 
- 4d: use `git branch` to check that you're on `name_of_new_branch`

5. Make changes

6. Push it back to your repo

**note** : You'll get a message `git push remote --` use this instead of the traditional `git push`

7. (Go back to the forked repo in GitHub) Click the **Compare & Pull Request** button.

- add a description to changes made in the pull request 

8. Click **Create pull request** to open a new pull request
