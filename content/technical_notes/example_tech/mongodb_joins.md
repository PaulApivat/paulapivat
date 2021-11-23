---
date: "2021-11-23T00:00:00+01:00"
draft: false
linktitle: Joins in Mongo
menu:
  example_tech:
    parent: Mongodb
    weight: 2
title: One-to-One Relations, Reference approach
toc: true
type: docs
weight: 2
---

## Joining

**Situation**: Among many possible relationships, this is a one-to-one relation between two tables using the "Reference" **instead** of the "Embedded Document" approach.

Here i'm joining a `bounties` collection with the `customers` collection through the `customer_id`:

```{python}
db.bounties.aggregate([
                        {$lookup: {from: "customers", 
                                   localField: "customer_id",
                                   foreignField: "customer_id", 
                                   as:"customerName"}
                        }
                      ])
```


For more content on data science, R, and Python [find me on Twitter](https://twitter.com/paulapivat).