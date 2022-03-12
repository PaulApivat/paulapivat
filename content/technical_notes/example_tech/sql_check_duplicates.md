---
date: "2022-03-10T00:00:00+01:00"
draft: false
linktitle: Check for duplicates
menu:
  example_tech:
    parent: SQL
    weight: 2
title: SQL Scripts to check for duplicates
toc: true
type: docs
weight: 2
---

## Check for Duplicate Rows in a Table

**Situation**: When running a data pipeline, it is often easier to check for data duplicates *after load* rather than during *extract* or *transform* (ETL).


**Context**: The script below checks for the `subgraph_bank_transactions` table, a pipeline that pulls in data from the $BANK token subgraph into a postgres database. This is part of the DAODash project. 

### Approach 1: `id` has duplicates

If the `query returned no data`, that means there are no duplicates.

In this case, `id` *does* have duplicates.

```{python}
SELECT
  id,
  COUNT(id)
FROM subgraph_bank_transactions 
GROUP BY id
HAVING COUNT(id) > 1;
```

| id    | count |
|-------|-------|
| 32165 | 2     |
| 31561 | 2     |
| 32657 | 4     |


### Approach 1a: `graph_id` does **not** have duplicates

```{python}
SELECT
  graph_id,
  COUNT(graph_id)
FROM subgraph_bank_transactions 
GROUP BY graph_id
HAVING COUNT(graph_id) > 1;
```

Result: `The query returned no data`


### Approach 2

```{python}
SELECT 
  a.*
FROM subgraph_bank_transactions a
JOIN (SELECT id, graph_id, amount_display, COUNT(*)
      FROM subgraph_bank_transactions 
      GROUP BY id, graph_id, amount_display
      HAVING COUNT(*) > 1) b 
ON a.id = b.id
AND a.graph_id = b.graph_id
AND a.amount_display = b.amount_display 
ORDER BY a.id

```


**Sources**:

[Learnsql.com](https://learnsql.com/cookbook/how-to-find-duplicate-rows-in-sql/)

[Chartio Data Tutorials](https://chartio.com/learn/databases/how-to-find-duplicate-values-in-a-sql-table/)



For more content on web3 data [find me on Twitter](https://twitter.com/paulapivat).