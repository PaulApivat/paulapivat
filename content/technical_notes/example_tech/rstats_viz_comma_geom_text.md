---
date: "2022-01-21T00:00:00+01:00"
draft: false
linktitle: Add comma separator in geom_text label
menu:
  example_tech:
    parent: Data Viz Tips
    weight: 2
title: Add comma separator in geom_text label
toc: true
type: docs
weight: 2
---

## Add comma separator in geom_text label

**Situation**: You want to create **two y-axes**. The left y-axis measures an amount, while the *right* y-axis converts the amount to a percentage. 

```{python}

df %>%
   ggplot(aes(x = var1, y = var2)) +
   geom_col() +
   geom_text(aes(label = scales::comma(Count_variable)), size = 15)
```



For more content on data, tech and web3 [find me on Twitter](https://twitter.com/paulapivat).

