---
date: "2020-08-27T00:00:00+01:00"
draft: false
linktitle: Calculating Percentiles
menu:
  example_tech:
    parent: Rstats Tip
    weight: 2
title: Calculating 25th, 50th and 75th Percentile of Column Values
toc: true
type: docs
weight: 2
---

## Calculating Percentiles

When we have a list of values in a column, how can we determine which values are under/over the 25th percentile, 50th percentile or 75th percentile?

Here the example are countries' *average percentages* of the population with, broadly speaking, ICT Skills as determine by the Sustainable Development Goals, [Indicator 4.4.1](https://unstats.un.org/wiki/display/SDGeHandbook/Indicator+4.4.1). 

There are two methods. First, manually calculating values for the 25th, 50th and 75th percentile with the `quantile()` function. 

```
# Country mean_values at 25th, 50th and 75th percentile 

data %>%
    select(GeoAreaName, Value, Sex, `Type of skill`, TimePeriod, Units) %>%
    rename(type_of_skill = `Type of skill`) %>%
    mutate(Value = as.numeric(Value)) %>%
    group_by(GeoAreaName) %>%
    summarize(
        mean_value = mean(Value)
    ) %>%
    mutate(
        min_mean = min(mean_value),
        iqr_25_percentile = quantile(mean_value, probs = c(0.25)),
        iqr_50_percentile = quantile(mean_value, probs = c(0.50)),
        iqr_75_percentile = quantile(mean_value, probs = c(0.75)),
        max_mean = max(mean_value)
    ) %>%
    arrange(desc(mean_value)) 
```

The second approach is to use the `ntile()` function:

```
# Creating bins using ntile()

data %>%
    select(GeoAreaName, Value, Sex, `Type of skill`, TimePeriod, Units) %>%
    rename(type_of_skill = `Type of skill`) %>%
    mutate(Value = as.numeric(Value)) %>%
    group_by(GeoAreaName) %>%
    summarize(
        mean_value = mean(Value)
    ) %>%
    mutate(
        mean_value_binned = ntile(mean_value, 4)
    ) %>%
    arrange(desc(mean_value))
```
