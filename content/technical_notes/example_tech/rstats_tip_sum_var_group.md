---
date: "2022-01-21T00:00:00+01:00"
draft: false
linktitle: Sum variable by group
menu:
  example_tech:
    parent: Rstats Tip
    weight: 2
title: Sum variable by group
toc: true
type: docs
weight: 2
---

## Sum variable by group

**Situation**: You want to sum by group. Assuming the dataframe has at least two columns, `category` and `value`. 

Check out [original answer](https://stackoverflow.com/questions/1660124/how-to-sum-a-variable-by-group) on stack overflow.


```{python}
library(dplyr)

df %>%
   group_by(category) %>%
   summarise(summed_value = sum(value))
```


For more content on data, tech and web3 [find me on Twitter](https://twitter.com/paulapivat).