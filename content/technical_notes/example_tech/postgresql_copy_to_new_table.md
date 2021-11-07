---
date: "2021-11-07T00:00:00+01:00"
draft: false
linktitle: Duplicate a table
menu:
  example_tech:
    parent: PostgreSQL
    weight: 2
title: Copy to new table
toc: true
type: docs
weight: 2
---

## Copy existing table to a new table

**Situation**: Useful to create 'tests' tables while testing a new data pipeline. You can preserve the original in case you need to Write to or Update a table.


```{python}
CREATE TABLE new_table AS
TABLE original_table
```



For more content on data science, R, and Python [find me on Twitter](https://twitter.com/paulapivat).