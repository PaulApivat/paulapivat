---
date: "2021-12-21T00:00:00+01:00"
draft: false
linktitle: Subtract Timestamps
menu:
  example_tech:
    parent: Mongodb
    weight: 2
title: Get time elapsed between two Timestamps
toc: true
type: docs
weight: 2
---

## Get time elapsed between two events 

**Situation**: You want to figure out how much time has elapsed between two timestamps.

The example below uses `$project` 3 consecutive times. 

**NOTE**: You have to convert timestamps (in string) to Date object with `$toDate` conversion. Another assumption is that timetsamps provide milliseconds so we need to subtract two dates by milliseconds, then convert those to either hours, days etc. 

```{python}
db.bounties.aggregate([
    {$match: {"$and": [{season: 2}, {customer_id: '905250069463326740'}, {status: "Completed"}]}},
    {$project: {_id: 0, _customer_id: "$customer_id", _title: "$title", _status: "$status", _createdAt: "$createdAt", _claimedAt: "$claimedAt", _submittedAt: "$submittedAt", _reviewedAt: "$reviewedAt"}},
    {$project: {_id: 0, customer_id: "$_customer_id", title: "$_title", status: "$_status", createdAt: {$toDate: "$_createdAt"}, claimedAt: {$toDate: "$_claimedAt"}, submittedAt: {$toDate: "$_submittedAt"}, reviewedAt: {$toDate: "$_reviewedAt"}}},
    {$project: {step_one: {$divide: [{$subtract: ["$claimedAt", "$createdAt"]}, 8.64e+7]} }}
]) 
```




For more content on data science, R, and Python [find me on Twitter](https://twitter.com/paulapivat).