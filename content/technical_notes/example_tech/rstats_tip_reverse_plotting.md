---
date: "2021-10-09T00:00:00+01:00"
draft: false
linktitle: Reverse Plotting
menu:
  example_tech:
    parent: Rstats Tip
    weight: 2
title: Reverse Plotting
toc: true
type: docs
weight: 2
---

## Top to Bottom

**Situation**: AFTER you display bar charts in a specific order because you successfully specified the [factor levels order](https://paulapivat.com/technical_notes/example_tech/rstats_tip_manually_order_factors/), you simply want to reverse the order (from top-to-bottom).

**Context**: I've created a specific factor order and simply want to reverse the order so what was on top is now on the bottom, using `scale_y_descrete(limits = rev())`, the `rev()` and `levels()` function are used in tandem to specific the reverse.

```{python}

new %>%
    count(name) %>%
    ggplot(aes(x = n, y = name, fill = name)) +
    geom_col() +
    geom_text(aes(label = n), hjust = -0.2, color = "white") +
    # reverse from top to bottom on the y-axis
    # note levels for a specific column new$name already set previously
    scale_y_discrete(limits = rev(levels(new$name))) 
```

## Right to Left

Works the same way, but using `scale_x_discrete`.


For more content on data science, R, and Python [find me on Twitter](https://twitter.com/paulapivat).