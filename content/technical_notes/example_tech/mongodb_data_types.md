---
date: "2021-11-23T00:00:00+01:00"
draft: false
linktitle: Data Types & Common Commands (Mongo)
menu:
  example_tech:
    parent: Mongodb
    weight: 2
title: Notable data types and common commands
toc: true
type: docs
weight: 2
---

## Data Types

**Situation**: These are specific for querying in Mongo. Good to keep these in mind:

- Text ("Max")
- Boolean (true)
- Integer (int32) (55)
- NumberLong (int64) (10000000000)
- NumberDecimal (12.99)
- ObjectId  (ObjectId("sfasd"))
- ISODate  (ISODate("2021-11-23"))
- Timestamp (Timestamp(11421532))
- Embedded Document ("a": {...})
- Array             ("a": [...])


## Common Commands in Mongo Shell

- To pull up quick descriptive stats for any database, type:
`db.stats()`
- To delete a collection, type:
`db.nameOfCollection.drop()`


For more content on data science, R, and Python [find me on Twitter](https://twitter.com/paulapivat).