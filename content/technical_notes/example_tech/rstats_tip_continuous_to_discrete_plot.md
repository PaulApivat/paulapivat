---
date: "2021-10-09T00:00:00+01:00"
draft: false
linktitle: Continuous to Discrete Plotting
menu:
  example_tech:
    parent: Rstats Tip
    weight: 2
title: Continuous to Discrete Plotting
toc: true
type: docs
weight: 2
---

## Continuous to Discrete Plotting

**Situation**: When plotting integers on the x-axis, it shows up in a ggplot like 2.5, 5, 7.5 and we want each bar to be plotted discretely (e.g. 1,2,3,4,5...)


```{python}

gov_partcipation %>%
    ggplot(aes(x = n, y = nn)) +
    geom_col(aes(fill = as.factor(n))) +
    geom_text(aes(label = nn), vjust = -0.5, color = "white") +
    # this sets the sequence from 1 to 10 with a break of 1
    # turns a continuous sequence (2.5) into a discrete one (1,2,3...)
    scale_x_continuous(breaks=seq(1,10, 1))
```



For more content on data science, R, and Python [find me on Twitter](https://twitter.com/paulapivat).