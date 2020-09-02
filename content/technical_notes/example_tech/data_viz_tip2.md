---
date: "2020-09-02T00:00:00+01:00"
draft: false
linktitle: Scatterplots & Marginal Distribution
menu:
  example_tech:
    parent: Data Viz Tips
    weight: 2
title: Visualize Scatterplots with Marginal Distribution using ggExtra
toc: true
type: docs
weight: 2
---

## Marginal Distribution with ggplot2 and ggExtra

The data in this example is from the UN [Statistics Division](https://unstats.un.org/sdgs/indicators/database/) Sustainable Development Goal, Indicator 4.4.1. 

Also check out the [r-graph-gallery.com](https://www.r-graph-gallery.com/277-marginal-histogram-for-ggplot2.html) for inspiration. 

Here's the breakdown:

1. Load Packages and Libraries

The key here is the `ggExtra` package.

```
install.packages('ggExtra')
library(ggExtra)
library(tidyverse)

```


2. Create a basic scatter plot

The key here is using `pivot_wider` to give all `Type of skill` their own columns. We'll then pick out specific columns (i.e., COPA, EMAIL, PCPR) to summarize, then plot on the x- and y- axes. 

```
# Basic Scatter Plot (color cluster by Gender)
p <- data %>%
    select(GeoAreaName, TimePeriod, Sex, `Type of skill`, Value) %>%
    group_by(GeoAreaName, TimePeriod, Sex, `Type of skill`, Value) %>%
    pivot_wider(names_from = `Type of skill`, values_from = Value) %>%
    mutate(
        COPA = as.numeric(COPA),
        EMAIL = as.numeric(EMAIL),
        PCPR = as.numeric(PCPR)
    ) %>%
    # Group by GeoAreaName, across TimePeriod, Sex
    group_by(GeoAreaName, Sex) %>%
    summarize(
        avg_COPA = mean(COPA, na.rm = TRUE),
        avg_EMAIL = mean(EMAIL, na.rm = TRUE),
        avg_PCPR = mean(PCPR, na.rm = TRUE)
    ) %>%
    ungroup() %>%
    ggplot(aes(x = avg_PCPR, y = avg_EMAIL, color = Sex)) + 
    geom_point()
```

3. Use `ggMarginal()` to create the marginal distribution along the side of the scatter plots. This is a function from the `ggExtra` package.

```
# Scatter Plot with Marginal Distribution
ggMarginal(p, type = 'histogram')
```

This particular chart is especially useful to highlight different distributions.


