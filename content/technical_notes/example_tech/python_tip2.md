---
date: "2020-09-23T00:00:00+01:00"
draft: false
linktitle: Creating and Looping through DataFrames
menu:
  example_tech:
    parent: Python Tips 
    weight: 1
title: Creating and Looping through DataFrames
toc: true
type: docs
weight: 1
---

## Creating and Looping through List of Tuples

If you come to Python from R, it's not immediately obvious how Lists, Dictionaries, Tuples, Series, then Loops help you do the things you can do in R. 

You can begin to connect the dots when you see that Lists of Tuples are the building blocks of DataFrames - available in both languages to handle tidy (tabular) data. 

# Lists

Lists are ordered and mutable collection of data. Below are lists of strings and integers.

```
name_list = ['paul', 'apivat', 'marvin', 'pim', 'milin']
int_list = [3,4,5,2,5,6,7,5]
```

# Tuples

Tuples, also collections, are ordered and immutable. But more related to the handling of data, tuples can be converted to DataFrames (using the Pandas library). Below, the List of Tuples (data) is converted into a DataFrame.

```
import pandas as pd

data = [
    ('r1', 'c1', 11, 11),
    ('r1', 'c2', 12, 12),
    ('r2', 'c1', 21, 21),
    ('r2', 'c2', 22, 22)
]

df = pd.DataFrame(data, columns=['R_Number', 'C_Number', 'Avg', 'Std'])
```

# Loops

You can loop through lists of strings and integers. 

```
int_list = [3,4,5,2,5,6,7,5]

for num in int_list:
    print(num)
    
name_list = ['paul', 'apivat', 'marvin', 'pim', 'milin']

for name in name_list:
    print(name)
```

# Looping through List of Tuples (DataFrame)

Just like you can loop through *any* collection, you can loop through a list of tuples - which means you can loop through DataFrames.

```
# Looping through column names
df = pd.DataFrame(data, columns=['R_Number', 'C_Number', 'Avg', 'Std'])

for col_names in df:
    print(col_names)
    
# Looping through a specific column
for items in df['R_Number']:
    print(items)
    
# Looping through a specific row
for items in df.iloc[1]:
    print(items)
```

That's the basic connection between python fundamental data structures and for-loop operations and data science. 