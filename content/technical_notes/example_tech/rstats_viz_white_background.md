---
date: "2021-12-01T00:00:00+01:00"
draft: false
linktitle: White background for ggplot
menu:
  example_tech:
    parent: Data Viz Tips
    weight: 2
title: Full white background for any ggplot chart
toc: true
type: docs
weight: 2
---

## Full white background for any ggplot chart

**Situation**: You want a clean white background for your chart in GGPLOT, however you don't remember how to adjust the theme().


```{python}

df %>%
  ggplot() +
  geom_col() +
  # white background
  theme_minimal() +
  theme(
        legend.position = "bottom",
        panel.grid.major = element_line(colour = "white"),
        panel.grid.minor = element_line(colour = "white")
  )
```

For more content on data science, R, and Python [find me on Twitter](https://twitter.com/paulapivat).

