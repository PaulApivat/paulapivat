---
date: "2021-02-05T00:00:00+01:00"
draft: false
linktitle: Anti Joins
menu:
  example_tech:
    parent: Rstats Tip
    weight: 2
title: Use Anti_Joins to Find Non-Matching Rows
toc: true
type: docs
weight: 2
---

## Finding Non-Matching Rows Before Joining Columns

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


