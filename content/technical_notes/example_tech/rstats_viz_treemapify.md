---
date: "2020-09-15T00:00:00+01:00"
draft: false
linktitle: Treemaps
menu:
  example_tech:
    parent: Data Viz Tips
    weight: 2
title: GGPlot Flavored Treemaps 
toc: true
type: docs
weight: 2
---

## Treemapify

There are several options for visualizing treemaps in R. This note focuses on  [Treemapify](https://cran.r-project.org/web/packages/treemapify/vignettes/introduction-to-treemapify.html), a package maintained by [David Wilkins](https://github.com/wilkox). 

I favor this approach over the `treemap` package because it is compatible with `ggplot2` and allows users to access its' functionality. 

Here's an example Treemap I created to visualize the dominant emotions displayed for the iconic 90's sitcom, Friends. You can find out more about the [Friends dataset here](https://github.com/rfordatascience/tidytuesday/blob/master/data/2020/2020-09-08/readme.md).

Other visualizations I created for the Friends project can also be found [here](https://github.com/PaulApivat/tidytuesday/tree/master/2020/friends).

Below, we can see that `geom_treemap`, `geom_treemap_subgroup_border` and `geom_treemap_subgroup_text` are layers in the ggplot2 system and works seamlessly with other staples like `scale_fill_manual`, `theme`, and `labs` within the ggplot2 package. 

Bottom line, it's easier to customize treemaps from the `treemapify` package than the `treemap` package. 

```
ggplot(friends_emo_tree, aes(area = n, label = speaker, subgroup = emotion)) +
    geom_treemap(aes(fill = emotion, alpha = n)) +
    geom_treemap_subgroup_border(color = 'white') +
    geom_treemap_subgroup_text(place = 'bottom', grow = T, alpha = 0.3, color = 'black',
                               min.size = 0) +
    geom_treemap_text(color = 'white', fontface = 'italic', place = 'centre', reflow = T) +
    scale_fill_manual(values = c('#FF4238', '#FFDC00', '#42A2D6', '#9A0006', '#FFF580', '#00009E')) +
    theme(
        plot.background = element_rect(fill = '#36454F'),
        legend.position = 'none',
        title = element_text(colour = 'white', family = 'Friends')
    ) +
    labs(
        title = 'The One with the Dominant Emotions',
        caption = 'Viz: @paulapivat | Data: #TidyTuesday'
    )
```