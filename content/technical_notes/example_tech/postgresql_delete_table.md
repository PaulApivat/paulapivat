---
date: "2021-11-07T00:00:00+01:00"
draft: false
linktitle: Delete a table
menu:
  example_tech:
    parent: PostgreSQL
    weight: 2
title: Delete a table
toc: true
type: docs
weight: 2
---

## Delete a table 

**Situation**: Sometimes when testing a pipeline, you mess up a table (e.g., append the wrong index), you just need to delete and start over.

```{python}
TRUNCATE TABLE public.name_of_table;
```



For more content on Data and DAOs [find me on Twitter](https://twitter.com/paulapivat).