---
date: "2020-08-27T00:00:00+01:00"
draft: false
linktitle: Order of Dplyr Functions
menu:
  example_tech:
    parent: Rstats Tip
    weight: 2
title: Does order of operation matter among dplyr functions?
toc: true
type: docs
weight: 2
---

## Data Wrangling: Does Order matter?

In short, yes, it matters. But when and where? 

Below are examples to highlight when function order matters and when it doesn't. The source for the raw data used in this illustration are from the United Nations' Statistics Division for [Sustainable Development Goals](https://unstats.un.org/sdgs/indicators/database/) (SDG) Indicators (Goal 4, Target 4.4). 

See also UN Statistics Wiki on [Indicator 4.4.1](https://unstats.un.org/wiki/display/SDGeHandbook/Indicator+4.4.1). 

```
# Example chain of functions to determine proportion of Thailand's population to have certain ICT skills in 2018

data %>%
    select(GeoAreaName, Value, Sex, `Type of skill`, TimePeriod) %>%
    rename(type_of_skill = `Type of skill`) %>%
    mutate(
        Value = as.double(Value)
    ) %>%
    filter(GeoAreaName == 'Thailand') %>% 
    filter(TimePeriod == 2018) %>% 
    group_by(type_of_skill) %>%
    summarize(
        mean_value = mean(Value),
        median_value = median(Value)
    ) %>%
    ungroup() %>%
    arrange(desc(mean_value))
```

Here, the `filter` functions are moved up to be before `rename` and `mutate`. The ordering here does **not** matter. 

```
data %>%
    select(GeoAreaName, Value, Sex, `Type of skill`, TimePeriod) %>%
    
    # putting filter before rename, mutate is fine
    filter(GeoAreaName == 'Thailand') %>% 
    filter(TimePeriod == 2018) %>% 
    
    rename(type_of_skill = `Type of skill`) %>%
    mutate(
        Value = as.double(Value)
    ) %>%
    group_by(type_of_skill) %>%
    summarize(
        mean_value = mean(Value),
        median_value = median(Value)
    ) %>%
    ungroup() %>%
    arrange(desc(mean_value))
```

Furthermore, we could even experiment with the `filter` function being before or after `select`. Here, ordering also does **not** matter. 

```
data %>%
    filter(GeoAreaName == 'Thailand') %>% 
    filter(TimePeriod == 2018) %>% 
    select(GeoAreaName, Value, Sex, `Type of skill`, TimePeriod) %>%
    rename(type_of_skill = `Type of skill`) %>%
    mutate(
        Value = as.double(Value)
    ) %>%
    group_by(type_of_skill) %>%
    summarize(
        mean_value = mean(Value),
        median_value = median(Value)
    ) %>%
    ungroup() %>%
    arrange(desc(mean_value))

```

Here is there order **does** matter. When we try to move the two `filter` functions below `group_by`, `summarize` and `ungroup`, the filtering does *not* work. By the time we get to `filter(GeoAreaName == 'Thailand')` in this example, GeoAreaName has been removed because we did *not* `group_by` `GeoAreaName`, so we get an error. 

```
# Running this code, we'll get the ERROR: Problem with `filter()` input `..1`. x object 'GeoAreaName' not found ℹ Input `..1` is 
# `GeoAreaName == "Thailand"`.

data %>%
    select(GeoAreaName, Value, Sex, `Type of skill`, TimePeriod) %>%
    relocate(Sex, Value, GeoAreaName) %>%
    rename(type_of_skill = `Type of skill`) %>%
    mutate(
        Value = as.double(Value)
    ) %>%
    # filter was previously here
    group_by(type_of_skill) %>%
    summarize(
        mean_value = mean(Value),
        median_value = median(Value)
    ) %>%
    ungroup() %>%
    # moving filter down below group_by & summarize() does not work
    filter(GeoAreaName == 'Thailand') %>% 
    filter(TimePeriod == 2018) %>% 
    arrange(desc(mean_value))
```

Finally, if we want to use `filter` on the *results* of the `mutate` function, we see that order **does** matter. By the time we get to the final `filter(Value < 10)`, the `Value` variable is no longer available to us because we did not `group_by` and `summarize` by Value (instead we created `mean_value` and `median_value`).

```

data %>%
    filter(GeoAreaName == 'Thailand') %>% 
    filter(TimePeriod == 2018) %>% 
    select(GeoAreaName, Value, Sex, `Type of skill`, TimePeriod) %>%
    rename(type_of_skill = `Type of skill`) %>%
    mutate(
        Value = as.double(Value)
    ) %>%
    # filtering for Values less than 10 does work here
    filter(Value < 10) %>%
    group_by(type_of_skill) %>%
    summarize(
        mean_value = mean(Value),
        median_value = median(Value)
    ) %>%
    ungroup() %>%
    arrange(desc(mean_value)) %>%
    # filter for Values less than 10 does not work down here
    filter(Value < 10)

```