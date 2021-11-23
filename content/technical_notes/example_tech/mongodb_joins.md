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

## Joining and Displaying Specific Fields

**Situation**: After joining two collections, you don't want to print everything out, only a specific set of key-value pairs.

Here I'm joining the `bounties` and `customers` collection and only want to display the `title` of the bounty and the `customerName` of the customer. (**note**: To omit the "_id", set it to `0`.)


```{python}
db.bounties.aggregate([{
    $lookup: {
        from: "customers", 
        localField: "customer_id", 
        foreignField: "customer_id", 
        as: "customerName"
    }
}, {
    $unwind: "$customerName"
}, {$project: {
        "_id": 0,
        "title": 1, 
        "customerName.customerName": 1
    } 
}
]);
```

## Pipelines in Mongo

The "pipeline" here is `$lookup` -> `$unwind` -> `$project`.

Use `$lookup` to create the join. Then `$unwind` to focus on the "result" of the `$lookup` ("as:..."). Finally, `$project` to specify the specific key-value pairs you want.


For more content on data science, R, and Python [find me on Twitter](https://twitter.com/paulapivat).