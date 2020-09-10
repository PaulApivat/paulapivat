---
date: "2020-09-10T00:00:00+01:00"
draft: false
linktitle: Reproducibility in Python
menu:
  example_tech:
    parent: Python Tips 
    weight: 1
title: Random Numbers & Reproducibility in Python
toc: true
type: docs
weight: 1
---

## Random Numbers with Numpy

`Numpy` has a sub-module called `random`. Technically both are of the 'module' class. `numpy.random` contains other methods like: `seed`, `set_state`, `standard_t` etc.

```
# Submodules

import numpy

print("numpy.random is a", type(numpy.random))
print("numpy is a", type(numpy))
print("it contains names such as...", dir(numpy.random)[-15:])
```

## Reproducibility

When using `numpy.random`, you can ensure reproducibility by accessing `numpy.random.seed(30)`, which mirrors #Rstats' `set.seed(30)` behavior. 

```
import random

numpy.random.seed(30)
rolls = numpy.random.randint(low=1, high=6, size=10)
rolls
```
