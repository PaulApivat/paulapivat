---
date: "2021-11-01T00:00:00+01:00"
draft: false
linktitle: SQL Check Columns Equality
menu:
  example_tech:
    parent: SQL
    weight: 2
title: Check Equality of Two Columns
toc: true
type: docs
weight: 2
---

## Check Equality of Two Columns 

**Situation**: There are two tables with two columns with different names. You want a simple script to check if the rows of those columns are equal, so the two tables can be joined. 

Here's the SQL script, taken from [this stackoverflow answer](https://stackoverflow.com/questions/1632792/how-do-i-compare-two-columns-for-equality-in-sql-server/1632831).

```{python}

SELECT 
  CASE WHEN COLUMN1 = COLUMN2 
    THEN '1' 
    ELSE '0' 
  END 
  AS MyDesiredResult
FROM Table1
INNER JOIN Table2 ON Table1.PrimaryKey = Table2.ForeignKey

```

Here's an application of this script used in **DAO Dash**. We are comparing two tables - `discord_user` and `discord_messages` by these two columns respectively:

1. `discord_user_id`
2. `author_user_id`

```{r}
SELECT 
  CASE WHEN discord_user_id = author_user_id 
    THEN '1'
    ELSE '0'
  END
  AS AreColumnsEqual
FROM discord_user d
INNER JOIN discord_messages m ON d.discord_user_id = m.author_user_id 
```


For more content on data science, R, and Python [find me on Twitter](https://twitter.com/paulapivat).