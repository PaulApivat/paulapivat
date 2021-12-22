---
date: "2021-12-22T00:00:00+01:00"
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

## No Hard Coding 

Reference this [stack overflow post](https://stackoverflow.com/questions/33194825/find-objects-created-in-last-week-in-mongo/46906862). To calculate the past 7-days we need to multiply: 7 days * 24 hours per day * 60 minutes per hour * 60 seconds per minute * 1000 milliseconds per second. 

**Note**: We want to get to milliseconds when dealing with timestamps. 


This example shows this method in `find()`.

```{python}
db.collection.find({
    timestamp: {
        $gte: new Date(new Date() - 7 * 60 * 60 * 24 * 1000)
    }
});
```

### Applied to Aggregation Pipeline

This query can be used in **Shell** as well as on **Mongo Chart/Compass**. Whne using in Mongo Chart, do **not** use Format to preserve: `new Date(new Date())`. This is from an example query for Bounty Board where we're querying bounties created in the past 7-days. 

```{python}
db.bounties.aggregate([
    {$match: {"$and": [{season: 2}, {customer_id: '834499078434979890'} ]}},
    {$project: {_id: 1, _customer_id: "$customer_id", _title: "$title", _status: "$status", _createdAt: "$createdAt", _claimedAt: "$claimedAt", _submittedAt: "$submittedAt", _reviewedAt: "$reviewedAt"}},
    {$project: {_id: 1, customer_id: "$_customer_id", title: "$_title", status: "$_status", createdAt: {$toDate: "$_createdAt"}, claimedAt: {$toDate: "$_claimedAt"}, submittedAt: {$toDate: "$_submittedAt"}, reviewedAt: {$toDate: "$_reviewedAt"}}},
    {$match: {"createdAt": {"$gt": new Date(new Date() - 7 * 60 * 60 * 24 * 1000)}}}
])
```


## Mongo Shell

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

## Hard Coded

Break out in case of emergency, generally avoid. 

```{python}
# Note: hardcoded ISODate

db.bounties.aggregate([
    {$match: {"$and": [{season: 2}, {customer_id: '834499078434979890'} ]}},
    {$project: {_id: 1, _customer_id: "$customer_id", _title: "$title", _status: "$status", _createdAt: "$createdAt", _claimedAt: "$claimedAt", _submittedAt: "$submittedAt", _reviewedAt: "$reviewedAt"}},
    {$project: {_id: 1, customer_id: "$_customer_id", title: "$_title", status: "$_status", createdAt: {$toDate: "$_createdAt"}, claimedAt: {$toDate: "$_claimedAt"}, submittedAt: {$toDate: "$_submittedAt"}, reviewedAt: {$toDate: "$_reviewedAt"}}},
    {$match: {"createdAt": {"$gte": ISODate("2021-12-15T00:00:00Z")}}}
]) 
```

For more content on data science, R, and Python [find me on Twitter](https://twitter.com/paulapivat).