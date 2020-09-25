---
date: "2020-09-25T00:00:00+01:00"
draft: false
linktitle: reactable
menu:
  example_tech:
    parent: Data Viz Tips
    weight: 2
title: Styling tables with reactable
toc: true
type: docs
weight: 2
---

## Setting up a barebones table with {reactable}

There are several `packages` to style your tables. This note will help you get setup with a basic table using the `reactable` package. With just a few lines of code, you can have a table with pagination and column sorting. 

The data for this note comes from [TidyTuesday 2020-09-22, "Himalayan Climbers"](https://github.com/rfordatascience/tidytuesday/blob/master/data/2020/2020-09-22/readme.md).

This note assumes that data has been wrangled and in `tibble` form, ready to be styled into a table.

# Sample Tibble

Here, I've saved my tibble of 20 rows and 3 columns in `df`.

```
> df

# A tibble: 20 x 3
   peak            attempts fail_rate
   <chr>              <dbl>     <dbl>
 1 Everest            21813     0.54 
 2 Cho Oyu             8890     0.570
 3 Ama Dablam          8406     0.479
 4 Manaslu             4593     0.621
 5 Dhaulagiri I        2592     0.789
 6 Makalu              2405     0.764
 7 Lhotse              2379     0.638
 8 Baruntse            2190     0.708
 9 Pumori              1780     0.706
10 Annapurna I         1669     0.821
11 Kangchenjunga       1385     0.682
12 Himlung Himal       1308     0.573
13 Annapurna IV         812     0.845
14 Putha Hiunchuli      738     0.599
15 Tilicho              670     0.781
16 Tukuche              462     0.753
17 Jannu                339     0.782
18 Langtang Lirung      338     0.84 
19 Makalu II            322     0.758
20 Nuptse               303     0.934

```

# Load Libraries
```
library(tidyverse)
library(reactable)
library(htmltools)

```

# Basic Table

The amazing thing is, with just this one line, you have a barebones table with **pagination** (with 20 rows, it shows 10 at a time; this can be adjusted) and  **sorting** for both columns. 

You can check out the rest of the repo [here](https://github.com/PaulApivat/tidytuesday/blob/master/2020/himalaya/exploratory.R)

```
reactable(df)
```

# Adding Bar Charts for Each Row

Of course, bare bones is not much to look at, so adding bar charts is essential for visually communicating quantities and percentages. However, you'll need to use the `htmltools` package to begin adding `div` to your chart. 

```
# Bar Charts can be added with a function 

bar_chart <- function(label, width = "100%", height = "14px", fill = "#00bfc4", background = NULL){
    bar <- div(style = list(background = fill, width = width, height = height))
    chart <- div(style = list(flexGrow = 1, marginLeft = "6px", background = background), bar)
    div(style = list(display = "flex", alignItems = "center"), label, chart)
}


# The bar_chart function is then inserted into the numeric columns

reactable(
    df,
    defaultSorted = "attempts",
    columns = list(
        peak = colDef(
            name = "Peaks"
        ),
        attempts = colDef(
            name = "Attempts (#)",
            defaultSortOrder = "desc",
            #format = colFormat(separators = TRUE),
            
            # Render Bar charts using a custom cell render function
            cell = function(value){
                width <- paste0(value * 100 / max(df$attempts), "%")
                # Add thousands separators
                value <- format(value, big.mark = ",")
                # Fix each label using the width of the widest number (incl. thousands separators)
                value <- format(value, width = 9, justify = 'right')
                bar_chart(value, width = width, fill = "#3fc1c9")
            },
            # And left-align the columns
            align = "left",
            # Use the operating system's default monospace font, and
            # preserve the white space to prevent it from being collapsed by default
            style = list(fontFamily = "monospace", whiteSpace = "pre")
        ),
        fail_rate = colDef(
            name = "Fail (%)",
            defaultSortOrder = "desc",
            #format = colFormat(percent = TRUE, digits = 1)
            
            # Render Bar charts using a custom cell render function
            cell = function(value){
                # Format as percentage with 1 decimal place
                value <- paste0(format(value * 100, nsmall = 1), "%")
                # Fix width here to align single and double-digit percentages
                value <- format(value, width = 5, justify = "right")
                bar_chart(value, width = value, fill = "#fc5185", background = "#e1e1e1")
            },
            # And left-align the columns
            align = "left",
            style = list(fontFamily = "monospace", whiteSpace = "pre")
        )
    )
)
```