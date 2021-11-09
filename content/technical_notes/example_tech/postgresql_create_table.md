---
date: "2021-11-09T00:00:00+01:00"
draft: false
linktitle: Create a table (establish connection)
menu:
  example_tech:
    parent: PostgreSQL
    weight: 2
title: Create a table to established connection with SQLAlchemy
toc: true
type: docs
weight: 2
---

## Create a Test Table then Insert data to establish a connection

**Situation**: You want to create a quick 'test' table via the `sqlalchemy` library in Python to establish a connection with your postgres db.


```{python}
# Create TEST table to confirm connection
db.execute(
    "CREATE TABLE IF NOT EXISTS films (title text, director text, year text)")

# Insert data
db.execute(
    "INSERT INTO films (title, director, year) VALUES ('Dune', 'Denis Villeneuve', '2021')")

# Read data
result_set = db.execute("SELECT * FROM films")
for r in result_set:
    print(r)
```



For more content on data science, R, and Python [find me on Twitter](https://twitter.com/paulapivat).