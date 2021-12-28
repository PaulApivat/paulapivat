---
date: "2021-12-28T00:00:00+01:00"
draft: false
linktitle: Loops & data structures 
menu:
  example_tech:
    parent: Python Tips 
    weight: 1
title: Looping through the 3 Main Data Structures in Python
image:
  caption: ""
  focal_point: ""
toc: true
type: docs
weight: 1
---

## Looping through the 3 Main Data Structures in Python

**Situation**: An introduction to loops and data structures.

In Python, three main ones are `lists`, `sets` and `dictionaries`. Here are some vignettes to show how they can be looped.

### Three ways to loop

```{python}
# set
data = {'Caleb', 'Jimmy', 'Sam', 'Monica', 'Caleb'}

# list
data = ['a', 'b', 'c', 'd', 'e']

# dictionary
data = {
    'Caleb': 5,
    'Jimmy': 10,
    'Sam': 12,
    'Monica': 20,
    'Caleb': 4
}


# direct
for i in data:
   print(i)
   
# range & len
for i in range(len(data)):
   print(i)


# enumerate
for i, letter in enumerate(data):
   print(i, letter)
   
```


