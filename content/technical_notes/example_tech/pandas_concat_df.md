---
date: "2022-02-08T00:00:00+01:00"
draft: false
linktitle: Merge several dataframes into one
menu:
  example_tech:
    parent: Pandas  
    weight: 1
title: Use concat to merge dataframes
image:
  caption: ""
  focal_point: ""
toc: true
type: docs
weight: 1
---

## Merge dataframes with concat

**Situation**: You want to combine multiple dataframes into one, to later push into a database. Something like `rbind` or `cbind` in R. 

You'll need to **reset index** twice to push into a database.

```{python}

# put dataframes into a list
frames = [df_24, df_63, df_305, df_492, df_878]

# use pandas and concat function to combine
concat_frames = pd.concat(frames)

# reset index twice
concat_frames_1 = concat_frames.reset_index()
concat_frames_2 = concat_frames_1.reset_index()

# OPTIONAL: select specific columns
specific_col_frames = concat_frames_2[[
    'level_0', 'Name', 'Address', 'Circle_Id']]

# OPTIONAL: rename some columns
# change column name 'level_0' -> 'index'
final_frames = specific_col_frames.rename(columns={'level_0': 'index'})
  

```

For more content on Data and DAOs [find me on Twitter](https://twitter.com/paulapivat).

