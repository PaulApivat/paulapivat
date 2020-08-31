---
date: "2020-08-26T00:00:00+01:00"
draft: false
linktitle: Tidy Data - pivot_wider
menu:
  example_tech:
    parent: Rstats Tip
    weight: 2
title: Using the pivot_wider() function
toc: true
type: docs
weight: 2
---

With [tidyr 1.0.0](https://www.tidyverse.org/blog/2019/09/tidyr-1-0-0/), there are several enhancements, one of which are `pivot_wider()` and `pivot_longer()`.

## Using pivot_wider()

In another [post](https://paulapivat.com/technical_notes/example_tech/rstats_tip2/), the `spread()` function was introduced as a way to observe the "tidy" principle of data formatting for analysis. 

The `pivot_wider()` function is an updated of `spread()` and is much more intuitive. Here's how it works:

```
# PIVOT_WIDER - even better than Spread

data %>%
    filter(GeoAreaName=="Morocco" | GeoAreaName=="Qatar") %>% 
    select(GeoAreaName, TimePeriod, Sex, `Type of skill`, Value) %>%
    group_by(GeoAreaName, TimePeriod, Sex, `Type of skill`, Value) %>%
    pivot_wider(names_from = `Type of skill`, values_from = Value) 
```

In this data set, `Type of skill` represents, broadly speaking, ICT Skills broken down into eight categories in this column. By using `pivot_wider()` each sub-category of ICT Skills gets it's **own** column, thus making the overall data frame *wider*.

## Using pivot_longer()

Conversely, there's also `pivot_longer` for the opposite effect. This next code chunk is part of my attempt for [TidyTuesday](https://github.com/rfordatascience/tidytuesday) ('Extinct Plants' for the week of 2020-08-18)

The `cols` parameter determines the range of columns to be changed from wide to long. The `names_to` parameter sets the new column name and `values_to` indicates the value of the new columns.

```
# PIVOT_LONGER - better than Gather

plants %>%
    select(binomial_name, threat_AA:action_NA) %>%
    pivot_longer(cols = threat_AA:action_NA, names_to = "action", values_to = "count") %>%
    ggplot(aes(x = binomial_name, y = action, fill = count)) +
    geom_tile() +
    theme(axis.text.x = element_text(angle = 90, hjust = 1))

```

