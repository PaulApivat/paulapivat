---
date: "2021-12-24T00:00:00+01:00"
draft: false
linktitle: Nested Documents in Arrays
menu:
  example_tech:
    parent: Mongodb
    weight: 2
title: Get values from nested array
toc: true
type: docs
weight: 2
---

## Grab values from nested arrays in a document

**Situation**: You want to figure out how much time has elapsed between two timestamps. But those timestamps are nested in arrays.

This is similar to the operation in [this post](https://paulapivat.com/technical_notes/example_tech/mongodb_subtract_timestamps/), with a key difference: the timestamps are nested within an **array of objects**, so we have to get those values out, then convert `$toDate`, then subtract. 

Note: We're **not** using `$unwind` here because we want multiple timestamps within the same document. 

```{python}
db.bounties.aggregate([
    {$match: {"$and": [{season: 2}, {customer_id: '905250069463326740'}, {status: "Completed"}]}},
    {$project: {"statusHistory.status": 1, "statusHistory.setAt": 1}},
    {$project: {draft: {$arrayElemAt: ["$statusHistory", 0]},  open: {$arrayElemAt: ["$statusHistory", 1]}, in_progress: {$arrayElemAt: ["$statusHistory", 2]}, in_review: {$arrayElemAt: ["$statusHistory", 3]}, completed: {$arrayElemAt: ["$statusHistory", 4]}}},
    {$project: {draft: {$toDate: "$draft.setAt"}, open: {$toDate: "$open.setAt"}, in_progress: {$toDate: "$in_progress.setAt"}, in_review: {$toDate: "$in_review.setAt"}, completed: {$toDate: "$completed.setAt"} }},
    {$project: {_id: 1, draft_to_open: {$divide: [{$subtract: ["$open", "$draft"]}, 3600000]}, open_to_progress: {$divide: [{$subtract: ["$in_progress", "$open"]}, 3600000]}, progress_to_review: {$divide: [{$subtract: ["$in_review", "$in_progress"]}, 3600000]}, review_to_completed: {$divide: [{$subtract: ["$completed", "$in_review"]}, 3600000]}}}
])
```


For more content on data [find me on Twitter](https://twitter.com/paulapivat).