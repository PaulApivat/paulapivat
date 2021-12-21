---
date: "2021-12-21T00:00:00+01:00"
draft: false
linktitle: Filter multiple conditions
menu:
  example_tech:
    parent: Mongodb
    weight: 2
title: Use $match to filter multiple conditions in Aggregation framework
toc: true
type: docs
weight: 2
---

## Filter multiple conditions using $match

**Situation**: At the beginning of an **Aggregation** pipeline, you may want to filter for multiple conditions before `$project`. 

Here's how you'd do it. Use `$and` operator with an *array* of conditions. This example is from the Bounty Board project. The query is filtering for `season`, `customer_id` and `status`:


```{python}
db.bounties.aggregate([
    {$match: {"$and": [{season: 2}, {customer_id: '905250069463326740'}, {status: "Completed"}]}},
    {$project: {_id: 0, customer_id: 1, "reward.amount": 1, "reward.currency": 1, status: 1}},
    {$group: {_id: "$customer_id", sum: {$sum: "$reward.amount"}}}
])
```

Incidentally, this pipeline is a useful pattern to remember: `$match` -> `$project` -> `$group`.


For more content on data science, R, and Python [find me on Twitter](https://twitter.com/paulapivat).