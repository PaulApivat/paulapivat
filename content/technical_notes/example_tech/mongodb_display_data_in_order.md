---
date: "2021-12-21T00:00:00+01:00"
draft: false
linktitle: Display data in order
menu:
  example_tech:
    parent: Mongodb
    weight: 2
title: Use $project to display data in a specific order
toc: true
type: docs
weight: 2
---

## Use $project to display data in a specific order

**Situation**: In an **Aggregation** pipeline, you may want to display data being `$project`-ed in a specific order. 

Here's how you'd do it. Put an *underscore* to the key names `_variable` and reference the actual variable in the value, `_variable: "$variable"`. See this [stack overflow](https://stackoverflow.com/questions/35254128/is-it-possible-to-get-the-fields-in-the-order-of-projection-in-aggregation-frame) for reference.


```{python}
# pay attention to first $project
db.bounties.aggregate([
    {$match: {"$and": [{season: 2}, {customer_id: '905250069463326740'}]}},
    {$project: {_id: 0, _customer_id: "$customer_id", _title: "$title", _status: "$status", _createdAt: "$createdAt", _claimedAt: "$claimedAt", _submittedAt: "$submittedAt", _reviewedAt: "$reviewedAt"}},
    {$project: {_id: 0, customer_id: "$_customer_id", title: "$_title", status: "$_status", createdAt: "$_createdAt", claimedAt: "$_claimedAt", submittedAt: "$_submittedAt", reviewedAt: "$_reviewedAt"}}
])
```

## Use $project twice to get original field names

**Situation**: After displaying in a specific order, you may want to retain the original field name. To do this, you'd use `$project` *again*. 

```{python}
# pay attention to second $project
db.bounties.aggregate([
    {$match: {"$and": [{season: 2}, {customer_id: '905250069463326740'}]}},
    {$project: {_id: 0, _customer_id: "$customer_id", _title: "$title", _status: "$status", _createdAt: "$createdAt", _claimedAt: "$claimedAt", _submittedAt: "$submittedAt", _reviewedAt: "$reviewedAt"}},
    {$project: {_id: 0, customer_id: "$_customer_id", title: "$_title", status: "$_status", createdAt: "$_createdAt", claimedAt: "$_claimedAt", submittedAt: "$_submittedAt", reviewedAt: "$_reviewedAt"}}
])
```


For more content on data science, R, and Python [find me on Twitter](https://twitter.com/paulapivat).