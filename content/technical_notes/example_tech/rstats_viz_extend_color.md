---
date: "2021-02-05T00:00:00+01:00"
draft: false
linktitle: Extend Color Palettes 
menu:
  example_tech:
    parent: Data Viz Tips
    weight: 2
title: Extend Color Palettes
toc: true
type: docs
weight: 2
---

## Extending the number of colors available in a palette

You might be using the `RColorBrewer` library and one of the palettes: Sequential, Qualitative or Diverging:

**Sequential** includes: Blues, BuGn, BuPu, GnBu, Greens, Greys, Oranges, OrRd, PuBu, PuBuGn, PuRd, Purples, RdPu, Reds, YlGn, YlGnBu YlOrBr, YlOrRd.

**Qualitative** includes: Accent, Dark2, Paired, Pastel1, Pastel2, Set1, Set2, Set3

**Diverging** includes: BrBG, PiYG, PRGn, PuOr, RdBu, RdGy, RdYlBu, RdYlGn, Spectral

These palettes 8-9 colors, at most 12. But what if you need more? Here'show to *extend* the palettes:

```
library(RColorBrewer)

number.colors <- 46
mycolors <- colorRampPalette(brewer.pal(8, "Set1"))(number.colors)

df %>%
   ggplot(aes(x=x, y=y)) +
   geom_boxplot() +
   geom_point() +
   scale_color_manual(values = mycolors)
```
