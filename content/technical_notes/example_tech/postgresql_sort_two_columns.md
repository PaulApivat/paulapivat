---
date: "2021-12-03T00:00:00+01:00"
draft: false
linktitle: Sort by Two Column
menu:
  example_tech:
    parent: PostgreSQL
    weight: 2
title: Sort by Two Column
toc: true
type: docs
weight: 2
---

## Sort by Two Column

**Situation**: You have a `Month` and `Day` column and you want to sort **both** in descending order.

Note: the last line.

```{python}
SELECT
  user_name,
  month,
  day,
  COUNT(content) AS content_count
FROM cte 
GROUP BY 1,2,3
ORDER BY month DESC, day DESC;
```


For more content on Data and DAOs [find me on Twitter](https://twitter.com/paulapivat).