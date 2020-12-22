---
date: "2020-12-22T00:00:00+01:00"
draft: false
linktitle: Installing Poetry 
menu:
  example_tech:
    parent: Python Tips 
    weight: 1
title: Setting up Poetry for Dependency Management
image:
  caption: ""
  focal_point: ""
toc: true
type: docs
weight: 1
---

## Setting up Poetry for Dependency Management

[Poetry](https://python-poetry.org/) is python packaging and dependency management made easy.

#### Poetry Install Script

```python
$ curl -sSL https://raw.githubusercontent.com/python-poetry/poetry/master/get-poetry.py | python3
```

## Sample Project

#### Create Sample Project

```python
$ poetry new how-long
```

#### Creating virtual environment

**NOTE:** This is done only **once**.

```python
$ poetry config virtualenvs.in-project true
$ poetry install
```

#### Launch virtual environment

```python
$ poetry shell 
```

#### Uninstall virtual environment

```python
$ poetry env remove 3.x
```


## Existing Project

1. Switch to master branch (if not already on)
2. git status
3. git pull
4. git checkout -b new_branch_name

**NOTE:** Assume that `poetry config virtualenvs.in-project true` has been run.

```python
$ poetry install
$ poetry shell 
```

Check `python` environment. 

