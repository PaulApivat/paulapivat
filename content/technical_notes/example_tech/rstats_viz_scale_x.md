---
date: "2020-09-15T00:00:00+01:00"
draft: false
linktitle: scale_x_continuous (ggplot2)
menu:
  example_tech:
    parent: Data Viz Tips
    weight: 2
title: Decimals to Integers
toc: true
type: docs
weight: 2
---

## Changing the x-axis from decimals to integers

When creating plots in `ggplot2` you'll often want to customize the x-axis so that values appear on a certain interval. In the example below, I wanted to change the intervals from 0.25, 0.50, 0.75 to 1,2,3,4 and so on. In this specific instance, I wanted *each* season of the show Friends to have its down tick on the x-axis (note: the show had ten seasons).

This operation changes the x-axis ticks from having decimals to being integers. 

```
library(ggplot)

ggplot(total_data, aes(x = season, y = episode, fill=imdb_rating)) +
    geom_tile() +
    scale_fill_gradient(low = '#FFF580', high = '#FF4238') +
    
    ## the seq() function defines the start and end numbers
    ## 'by =' indicates the desired interval
    scale_x_continuous(breaks = seq(1,10, by = 1)) + 
    
    theme_classic()
```