---
date: "2021-03-03T00:00:00+01:00"
draft: false
linktitle: Circular Dendrogram 
menu:
  example_tech:
    parent: Data Viz Tips
    weight: 2
title: Circular Dendrogram
toc: true
type: docs
weight: 2
---

## Creating a Circular Dendrogram with ggraph

I tried creating a [Circular Dendrogram](https://www.r-graph-gallery.com/339-circular-dendrogram-with-ggraph.html) using reproducible code from R Graph Gallery. 

However, the code on the website has some issues so I submitted a [pull request to fix it](https://github.com/holtzy/R-graph-gallery/pull/34).

The code below, for the most part, match the original on R Graph Gallery, except 4 lines of code that were updated to fix the `geom_node_text` issue:

```python
# libraries
library(ggraph)
library(igraph)
library(tidyverse)
library(RColorBrewer) 

# create data.frame
d1=data.frame(from="origin", to=paste("group", seq(1,10), sep=""))
d2=data.frame(from=rep(d1$to, each=10), to=paste("subgroup", seq(1,100), sep="_"))
edges=rbind(d1, d2)

# create vertices
vertices = data.frame(
    name = unique(c(as.character(edges$from), as.character(edges$to))) , 
    value = runif(111)
) 

vertices$group = edges$from[ match( vertices$name, edges$to ) ]


vertices$id=NA
myleaves=which(is.na( match(vertices$name, edges$from) ))
nleaves=length(myleaves)
vertices$id[ myleaves ] = seq(1:nleaves)

# change the angle of the geom_node_text
# angle and hjust must be consistent

vertices$angle = 360 / nleaves * vertices$id + 110  # adjust angle calculation

vertices$hjust<-ifelse( vertices$angle < 291, 1, 0) # adjust hjust

vertices$angle<-ifelse(vertices$angle < 291, vertices$angle+180, vertices$angle) # adjust where 180 is added

# crate mygraph object (dendrogram)
mygraph <- graph_from_data_frame( edges, vertices=vertices )

# plot the dendrogram
ggraph(mygraph, layout = 'dendrogram', circular = TRUE) + 
    geom_edge_diagonal(colour="grey") +
    scale_edge_colour_distiller(palette = "RdPu") +
    geom_node_text(aes(x = x*1.15, y=y*1.15, filter = leaf, label=name, angle = angle, hjust=hjust, colour=group), size=2.7, alpha=1) +
    geom_node_point(aes(filter = leaf, x = x*1.07, y=y*1.07, colour=group, size=value, alpha=0.2)) +
    scale_colour_manual(values= rep( brewer.pal(9,"Paired") , 30)) +
    scale_size_continuous( range = c(0.1,10) ) +
    theme_void() +
    theme(
        legend.position="none",
        plot.margin=unit(c(0,0,0,0),"cm"),
    ) +
    expand_limits(x = c(-1.3, 1.3), y = c(-1.3, 1.3)) +
    coord_flip()  # add coord_flip

```

For more content on data science, R, and Python [find me on Twitter](https://twitter.com/paulapivat).

