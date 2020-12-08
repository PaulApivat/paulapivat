---
date: "2020-12-08T00:00:00+01:00"
draft: false
linktitle: Format Dates
menu:
  example_tech:
    parent: Rstats Tip
    weight: 2
title: Formating Dates with Lubridate
toc: true
type: docs
weight: 2
---

## Changing Date formats with Lubridate

For every data visualization project that involves using `dates` on one of the axes, I always find myself having to re-format the date so that visualization "works".

Here's a workflow that is recommended at the **start** of any data visualization project. 

```
library(tidyverse)
library(lubridate) # handling dates

df %>%
    # handling date first
    mutate(
        date = original_date_variable %>% ymd(),
        year = date %>% year(),
        month = date %>% month(),
        day = date %>% day(),
        year_month = make_datetime(year, month) # combine year & month
    ) 
```

