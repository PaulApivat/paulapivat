---
date: "2021-12-31T00:00:00+01:00"
draft: false
linktitle: Joining Sets
menu:
  example_tech:
    parent: Python Tips 
    weight: 1
title: Ways to Join Sets
image:
  caption: ""
  focal_point: ""
toc: true
type: docs
weight: 1
---

## 4 Operations to Join Sets

**Situation**: You have sets containing strings, you can join, find intersections, or differences and symmetric differences.

### 4 Operations

```{python}
my_drinks = {'grape', 'cola', 'black cherry'}
her_drinks = {'lime', 'cola'}

# position if not retained in a set
print(my_drinks, her_drinks)

# 4 operations to know about

# union
print("Union", my_drinks | her_drinks)


# intersection
print("Intersection", my_drinks & her_drinks)

# difference
print("Difference", my_drinks - her_drinks)


# symmetric difference
print("Symmetric Difference", my_drinks ^ her_drinks)
   
```

