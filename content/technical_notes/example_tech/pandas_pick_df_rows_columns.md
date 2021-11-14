---
date: "2021-11-14T00:00:00+01:00"
draft: false
linktitle: Picking df cells in Pandas
menu:
  example_tech:
    parent: Pandas  
    weight: 1
title: Picking columns, rows in Pandas
image:
  caption: ""
  focal_point: ""
toc: true
type: docs
weight: 1
---

## Picking columns, rows in Pandas

Note: To identify a specific cell, it's good to have at least a unique `id` column to reference.

This part grabs the specific row: `df.loc[df['id'] == number]`

This part grabs the specific column: `['column_name']`

```{python}
# generic
df.loc[df['id'] == number]['column_name']

# specific
concat_frames_4.loc[concat_frames_4['id'] == 1983]['choice']
```

For more content on Data and DAOs [find me on Twitter](https://twitter.com/paulapivat).

