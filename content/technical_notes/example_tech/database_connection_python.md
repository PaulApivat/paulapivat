---
date: "2022-02-26T00:00:00+01:00"
draft: false
linktitle: Database Connection with Python
menu:
  example_tech:
    parent: Database
    weight: 2
title: Postgres DB Connection with Python
toc: true
type: docs
weight: 2
---

## Postgres Database Connection

**Description**: In this example, we'll use Python, SQLAlchemy to connect to a Postgres database. Moreover, we'll use Python context managers for cleaner [functions](https://paulapivat.com/technical_notes/example_tech/python_functions/).

This example is from a **DAODash pipeline** to update the `bank_subgraph_transactions` table. 

**Note**: This example does not separate business logic from database connection, but has them in the same file.

```{python}
# libraries for postgres connection
import os
from sqlalchemy import create_engine
from sqlalchemy import text

# using python-dotenv to access environment variables
from dotenv import load_dotenv
load_dotenv()


# db_string = 'postgresql://user:password@localhost:port/mydatabase'

db_string = os.environ.get('DB_STRING')

@contextlib.contextmanager
def get_postgres_conn(db_string):
    """description:
    Context manager to automatically close DB connection
    Retrieve credentials from environment variables (.env)
    yield: database connection
    note: close database connection
    """

    db = create_engine(db_string)

    yield db

    db.dispose()


# use get_postgres_conn context manager
# note: interaction w/ subgraph_bank_transactions table from postgres db
with get_postgres_conn(db_string) as conn:
    result = conn.execute(
        text("SELECT MAX(tx_timestamp) AS max_tx_timestamp, MAX(id) AS max_id FROM subgraph_bank_transactions")
    )
    for row in result:
        max_tx_timestamp = row.max_tx_timestamp
        max_id = row.max_id
        print("new max_tx_timestamp: ", max_tx_timestamp)
        print("new max_id: ", max_id)

```




For more content on web3 data [find me on Twitter](https://twitter.com/paulapivat).