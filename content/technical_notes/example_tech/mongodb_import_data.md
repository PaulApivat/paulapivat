---
date: "2021-11-23T00:00:00+01:00"
draft: false
linktitle: Importing Data
menu:
  example_tech:
    parent: Mongodb
    weight: 2
title: Importing a Collection
toc: true
type: docs
weight: 2
---

## Importing Data

**Situation**: You need to import an array of objects (i.e., a collection) in Mongo.

Assuming you have the collection already stored in a JSON file, here's the `shell` command to import:

```{python}
$ mongoimport file-name.json -d databaseName -c collectionName --jsonArray --drop
```

The breakdown is:

```{python}
-d databaseName
-c collectionName
--jsonArray  # importing an array of objects
--drop       # drop existing collection of same name to use most recent (optional)
```

For more content on data science, R, and Python [find me on Twitter](https://twitter.com/paulapivat).