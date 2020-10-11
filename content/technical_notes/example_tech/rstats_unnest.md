---
date: "2020-10-11T00:00:00+01:00"
draft: false
linktitle: Unnest JSON data
menu:
  example_tech:
    parent: Rstats Tip
    weight: 2
title: Reading and manipulating nested data
toc: true
type: docs
weight: 2
---

## Ways of handling nested data

Recently, I downloaded JSON data from BigQuery and had to make sense of the data. This starts with getting the data into tabular form.

Here are the libraries I used:

```
library(jsonlite)
library(tidyverse)
```

First, read in JSON data. Once read in, we check its class type to see that its a list. We'll want to get it into a data frame.

```
# read data out into Large list (321 elements, 2.4 Mb)
# each row is *another* list

funnel <- lapply(readLines("bq-mixpanel-funnel.json"), fromJSON)

# "list" class
class(funnel)
```

After searching online, three approaches continually resurfaced.

First, using `unlist()` and converting into `matrix()` before wrapping *that* in a `data.frame()`:

```
# Approach 1: convert to matrix, array

unlist_funnel <- matrix(unlist(funnel), byrow = TRUE, ncol = length(funnel[[1]]))
rownames(unlist_funnel) <- names(funnel)
as.data.frame(unlist_funnel) %>% view()
```

These next approaches get us closer (**note**: I know from interacting with the data in BigQuery that there *should* be 321 rows):

```
# Approach 2: Convert list to data frame

df <- data.frame(matrix(unlist(funnel), nrow = length(funnel), byrow = TRUE))
df2 <- data.frame(matrix(unlist(funnel), nrow = length(funnel), byrow = FALSE))
df3 <- data.frame(matrix(unlist(funnel), nrow = 321, byrow = TRUE), stringsAsFactors = FALSE)
```

The next approach is to use `lapply()`

```
#works but everything is on one column

unlist(lapply(funnel, c)) %>% view() 

# this makes everything a list, but we want everything into a vector
t(lapply(funnel, c)) %>% view() 
```

Finally, the approach that worked best, in this particular case was `sapply()`. This functions turns things into vector, which can then be converted into a dataframe:

```
#still the ideal, this works because 'c' is used ot combine lists

t(sapply(funnel, c)) %>% view()  
```