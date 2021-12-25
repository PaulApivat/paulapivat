---
date: "2021-12-25T00:00:00+01:00"
draft: false
linktitle: Upload DataFrame (SQLAlchemy)
menu:
  example_tech:
    parent: PostgreSQL
    weight: 2
title: Writing dataframe to postgresql database 
toc: true
type: docs
weight: 2
---

## Writing dataframe to postgresql database 

**Situation**: You need to write data from an API endpoint into postgres. You can use SQLAlchemy and Python for the job. A benefit of this method is that you don't need to rely on Excel. 

In the example below, we query data from an API endpoint (Coordinape), and we've setup `.env` to store authorization credentials. We used pandas to transform the data *before* loading to postgres. 

Now we want to use SQLAlchemy to write the dataframe to postgres. 

[Source](https://pythontic.com/pandas/serialization/postgresql?fbclid=IwAR0fzgR7wBspGl6mAqWyt8N2lDwS7a36MvWLoFstDJBQDJsnBVMo7J0cwGc)

```{python}
# libraries for database connection
import time
from sqlalchemy import create_engine
import psycopg2

# NOTE: By now a dataframe is ready for writing to postgres
# here the dataframe = concat_frames_5

# Use SQLAlchemy
# INPUT DATABASE CONNECTION STRING
conn_string = os.environ.get('CONN_STRING')
print(conn_string)

alchemyEngine = create_engine(conn_string)
postgreSQLConnection = alchemyEngine.connect()

postgreSQLTable = "coordinape_rounds_3"

try:
    frame = concat_frames_5.to_sql(
        postgreSQLTable, postgreSQLConnection, if_exists='fail')
except ValueError as vs:
    print(vx)
except Exception as ex:
    print(ex)
else:
    print(
        f"PostgreSQL Table {postgreSQLTable} has been created successfully.")
finally:
    postgreSQLConnection.close()

```




For more content on Data and DAOs [find me on Twitter](https://twitter.com/paulapivat).