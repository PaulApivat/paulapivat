---
date: "2021-12-05T00:00:00+01:00"
draft: false
linktitle: Non-Matching Rows between Two Columns
menu:
  example_tech:
    parent: Rstats Tip
    weight: 2
title: Approaches to finding Non-Matching Rows between Two Columns
toc: true
type: docs
weight: 2
---

## Approaches to finding non-matching rows between 2 columns

**Situation**: You have two email lists - a full and already_sent. The latter is a subset of the former.

The **first** condition to test is that the `full` and `already_sent` lists are mutually exclusive. 

**Second**, the examples below assume the columns are in different dataframes. 

**Third**, you generally put the longer column first in a function.

Then, you want to find non-matching rows.

## Demo Two Tables

Table 1: a1

| a | b |
|---|---|
| 1 | a |
| 2 | b |
| 3 | c |
| 4 | d |
| 5 | e |

Table 2: a2

| a | b |
|---|---|
| 1 | a |
| 2 | b |
| 3 | c |

```{python}
# generate two dataframes to demo
a1 <- data.frame(a = 1:5, b=letters[1:5])
a2 <- data.frame(a = 1:3, b=letters[1:3])
```

## Dplyr: anti_join

The `anti_join` function comes with `dplyr`. You will find non-matching rows between 2 tables;


```{python}
library(dplyr)

anti_join(a1, a2, by = 'b')
```

## Dplyr: setdiff

This returns the non-matching rows by individual characters. You'll need to wrap the output in a `data.frame()`. This solution is not as clean as a `anti_join` or the `setDT` down below.


```{python}
library(dplyr)

# individual characters
setdiff(a1$b, a2$b)

# wrapped in a dataframe
data.frame(setdiff(a1$b, a2$b))
```



## Data.Table: setDT

```{python}
library(data.table)

setDT(a1)[!a1$b %chin% a2$b]
```

## Double check in Excel

Check if values column `a1` exists in column `a2`. You're going to create a middle column between the two with values of either `Exist`, where both emails exists or `Not Exist`, where one column is missing. 

Assuming `a1` starts on cell A4.
Assuming `a2` starts on cell C4.

In the scenario of Full Email List vs Emails Sent case, `a1` (Full Email List) had 4286 rows, while `a2` (Emails Sent) had 2373 rows. Here's the Excel function:

```{python}
# Excel
=IF(ISERROR(VLOOKUP(C2,$A$2:$A$4286, 1, FALSE)),"Not Exist","Exist" )
```
[source](https://www.extendoffice.com/documents/excel/3040-excel-check-if-value-is-in-another-column.html)

## Finding Non-Matching Rows: anti_join()

We often need to join two columns from different data frames. Rows to be joined are *assumed* to have the same value. 

Even different casing means those values will not be joined. For example: "Nigeria" and "NIGERIA" will not be joined. 

It's particularly useful to know which values are **not** in sync when you have a list of countries and you want to join with one of the `map` libraries (e.g., `ggmap`). If the country is spelt differently, the join doesn't happen.

Enter anti_join:

```
world_map1 <- world_map %>%
    mutate(id = region)

df1 <- df %>%
    mutate(id = country)
    
# use anti_join to figure out which rows are not aligned
anti_join(world_map1, df1, by = "id")
```


