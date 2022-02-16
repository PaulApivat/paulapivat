---
date: "2022-02-16T00:00:00+01:00"
draft: false
linktitle: SQL and Pandas
menu:
  example_tech:
    parent: SQL
    weight: 2
title: Use pandas with SQL queries
toc: true
type: docs
weight: 2
---

## Pandas and SQL

**Situation**: You want to further manipulate SQL data in Python. You can use `SQLAlchemy` and `pandas` to achieve this. 



```{python}
# Load Libraries
import pandas as pd
from sqlalchemy import create_engine

# for python-dotenv method
import os
from dotenv import load_dotenv   
load_dotenv()    

# db connection
sql_db_connection = os.environ.get("SQL_DB_CONNECTION")  # os.environ.get('postgres:///data.db')

# Create database engine
engine = create_engine(sql_db_connection)

# write query to get records with filter
query = """SELECT *
             FROM table 
            WHERE borough = 'BRONX'
               OR borough = 'BROOKLYN'
"""

# query the database
bronx_query = pd.read_sql(query, engine)

print(bronx_query.unique())  # print distinct records
print(bronx_query.shape())   # print shape of data
```


For more content on data in web3 [find me on Twitter](https://twitter.com/paulapivat).