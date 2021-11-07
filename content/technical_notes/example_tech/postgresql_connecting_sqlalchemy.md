---
date: "2021-11-07T00:00:00+01:00"
draft: false
linktitle: Establish connection (SQLAlchemy)
menu:
  example_tech:
    parent: PostgreSQL
    weight: 2
title: Connecting to postgresql database 
toc: true
type: docs
weight: 2
---

## Connecting to postgresql database with python

**Situation**: You need to establish connection to an existing table in your postgresql database in order to build a data pipeline into it. 

I use `sqlalchemy` to work with existing tables in postgresql. In this project, I connected to a GraphQL API endpoint with `requests` and the `json` library is needed to work with JSON and `pandas` for dataframes. 

```{python}
import sqlalchemy
from sqlalchemy import create_engine
from sqlalchemy import text

import requests
import json
import pandas as pd
from pprint import pprint
```

## Making a connection

With `sqlalchemy` we use the `create_engine()` function. Here we're reading a table and doing some data manipulation:

```{python}
db_string = 'postgresql://user:password@localhost:port/mydatabase'
db = create_engine(db_string)

# once a database connection is established, we can select pieces of data we want from a table:

# Query existing postgres table: stg_subgraph_bank
# read from stg_subgraph_bank to select MAX (tx_timestamp)
# then, set to variable max_tx_timestamp

with db.connect() as conn:
    result = conn.execute(
        text("SELECT MAX(tx_timestamp) AS max_tx_timestamp, MAX(id) AS max_id FROM stg_subgraph_bank_1"))
    for row in result:
        max_tx_timestamp = row.max_tx_timestamp
        max_id = row.max_id
        print("new max_tx_timestamp: ", max_tx_timestamp)
        print("new max_id: ", max_id)

```


For more content on Data and DAOs [find me on Twitter](https://twitter.com/paulapivat).