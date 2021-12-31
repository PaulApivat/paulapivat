---
date: "2021-12-31T00:00:00+01:00"
draft: false
linktitle: Iterating and Updating data structures 
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
# grab index and key, not value
# remove duplicate keys
for i, letter in enumerate(data):
   print(i, letter)
   
```

## Iterating through Dictionaries

```{python}
data = {
    'Caleb': 5,
    'Jimmy': 10,
    'Sam': 12,
    'Monica': 20,
    'Caleb': 4
}

# .items() method returns key & values
for key, val in data.items():
    print(key, val)
    
# enumerate gets index & key (no values)
for index, value in enumerate(data):
    print(index, value)
```

## Updating Dictionaries

You have a dictionary with key, value pairs, you want to update values of *existing* keys and find new keys and determine value. 

```{python}
drinks_in_storage = {
    "grape": 5, 
    "black cherry": 3, 
    "cola": 4
}

# update with List of Lists
drink_purchased = [['grape', 3], ['black cherry', 1], ['cola', 5], ['orange', 12]]

# can also update with List of Sets
drink_purchased = [('grape', 3), ('black cherry', 1), ('cola', 5), ('orange', 12)]

# checking if a key is present before updating value
for drink in drink_purchased:
    if drinks_in_storage.get(drink[0]):
        drinks_in_storage[drink[0]] += drink[1]
    else:
        drinks_in_storage[drink[0]] = drink[1]

# see that drinks_in_storage added 'orange':12 to the dictionary
print(drinks_in_storage) 
```
