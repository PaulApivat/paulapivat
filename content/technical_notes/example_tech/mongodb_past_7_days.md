---
date: "2021-12-17T00:00:00+01:00"
draft: false
linktitle: Get past 7-days data
menu:
  example_tech:
    parent: Mongodb
    weight: 2
title: Retrieving past 7-days data
toc: true
type: docs
weight: 2
---

## Retrieving past 7-days data

**Situation**: You want to query a collection to get documents that were generated in the past 7 days. 

Note: This operation can be done in **Mongo Shell**, need to find another way to do it in Mongo Compass / Atlas. 

```{python}
# Compute the time 7 days ago to use in filtering the data
# across all customers
var d = new Date();
d.setDate(d.getDate()-7);

db.bounties.aggregate([
    {$match: {season: 2, customer_id: '905250069463326740'}},
    {$project: {_id: 1, title: 1, customer_id: 1, createdAt: {$toDate: "$createdAt"}}},
    {$match: {'createdAt': {$gt: d}}},
    {$unwind: '$createdAt'},
    {$match: {'createdAt': {$gt: d}}}
]).pretty()
```

For more content on data science, R, and Python [find me on Twitter](https://twitter.com/paulapivat).