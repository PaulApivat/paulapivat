---
date: "2021-11-07T00:00:00+01:00"
draft: false
linktitle: Select Range of Rows
menu:
  example_tech:
    parent: PostgreSQL
    weight: 2
title: Select a range of rows
toc: true
type: docs
weight: 2
---

## Select a range of rows 

**Situation**: This is most useful if your data table does not have a primary key. You can use the table's natural index.

Here is a way to select a range of rows. **OFFSET** value is the number of rows to skip (here skipping `20500` rows before returning any rows). **ALL** indicates the max number of rows to return.

```{python}
SELECT * FROM table
LIMIT ALL OFFSET 20500
```

If you wanted to start on row 15 and only return 10 rows, the query would be:

```{python}
SELECT * FROM table
LIMIT 10 OFFSET 15
```


For more content on data science, R, and Python [find me on Twitter](https://twitter.com/paulapivat).