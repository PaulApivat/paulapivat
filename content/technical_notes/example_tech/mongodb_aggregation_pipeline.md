---
date: "2021-12-21T00:00:00+01:00"
draft: false
linktitle: Aggregation Pipeline
menu:
  example_tech:
    parent: Mongodb
    weight: 2
title: Exploring Mongo's Aggregation Pipeline
toc: true
type: docs
weight: 2
---

## Pipelines in Mongo

Previously, we looked at a "pipeline" `$lookup` -> `$unwind` -> `$project`.

Use `$lookup` to create the join. Then `$unwind` to focus on the "result" of the `$lookup` ("as:..."). Finally, `$project` to specify the specific key-value pairs you want.

Note: In **Mongo Compass Charts**, you can *re-use* either `queries` `{}` or `aggregation` `[]` entered into the query field

Here's an example to join the `bounties` and `customers` collection from the Bounty Board project:

```{python}
db.bounties.aggregate([
  {
    $lookup: {
      from: "customers",
      localField: "customer_id",
      foreignField: "customer_id",
      as: "customerName",
    },
  },
  {
    $unwind: "$customerName",
  },
  {
    $project: {
      _id: 0,
      title: 1,
      "customerName.customerName": 1,
    },
  },
]);
```

## Aggregation Framework

Let's breakdown aggregation pipelines further. Here are some variations:

1. match-group-sort
2. match-sortByCount
3. match-sort-project
4. match-project-group-sort
5. match-project-group
6. project-sort
7. project-group
8. (multiple $project) to project in a specific order

This shows the flexibility of Aggregation pipelines and different ways to querying and displaying data.

Here are codes associated with the above examples:

1. match-group-sort

```{python}
# seasonal filter
# group by Bounty Creator, number of bounties per creator
# average reward posted
# sort by total bounties per person

db.bounties
  .aggregate([
    { $match: { season: 2 } },
    {
      $group: {
        _id: { creator_name: "$createdBy.discordHandle" },
        num_bounties_created: { $sum: 1 },
        avg_reward_amt: { $avg: "$reward.amount" },
      },
    },
    { $sort: { num_bounties_created: -1 } },
  ])
  .pretty();
```


2. match-sortByCount

```{python}
# short cut to grouping by Bounty Creator
# can apply to Claimers and Submitters
db.bounties.aggregate([
  { $match: { season: 2 } },
  { $sortByCount: "$createdBy.discordHandle" },
]);
```
3. match-sort-project

```{python}
# seasonal filter
# sort by createdAt date, starting with most recent
# project/display only title and createdAt date
# note: can use '$toDate' instead of '$convert, input, to'
db.bounties.aggregate([
    { $match: { season: 2 } },
    { $sort: { createdAt: -1 } },
    {
      $project: {
        _id: 0,
        title: 1,
        createdAt: { $convert: { input: "$createdAt", to: "date" } },
      },
    },
  ]).pretty();
```
4. match-project-group-sort

```{python}
# seasonal filter
# display avg_reward bountied per week
# group by week
# sort by week, by week in descending order
db.bounties.aggregate([
  { $match: { season: 1 } },
  {
    $project: {
      _id: 0,
      season: 1,
      "reward.amount": 1,
      createdAt: { $toDate: "$createdAt" },
    },
  },
  {
    $group: {
      _id: { week: { $isoWeek: "$createdAt" } },
      avg_reward: { $avg: "$reward.amount" },
    },
  },
  { $sort: { week: -1 } },
]);
```
5. match-project-group

Here you can filter by multiple conditions, then project the columns you want to see and then group to get a summation.

This was useful to tally total values in Bounties given and/or claimed. 

```{python}
db.bounties.aggregate([
    {$match: {"$and": [{season: 2}, {customer_id: '905250069463326740'}, {status: "Completed"}]}},
    {$project: {_id: 0, customer_id: 1, "reward.amount": 1, "reward.currency": 1, status: 1}},
    {$group: {_id: "$customer_id", sum: {$sum: "$reward.amount"}}}
])
```



6. project-sort

```{python}
# display subset of fields in bounties
# then, sort by reward amount
db.bounties.aggregate([
  {
    $project: {
      _id: 0,
      season: 1,
      title: 1,
      "reward.amount": 1,
      "reward.currency": 1,
    },
  },
  { $sort: { "reward.amount": -1 } },
]);
```
7. project-group

```{python}
# group bounty by Season,
# average reward committed
db.bounties.aggregate([
  { $project: { _id: 0, season: 1, "reward.amount": 1 } },
  {
    $group: {
      _id: { bounty_season: "$season" },
      avg_reward: { $avg: "$reward.amount" },
    },
  },
]);
```
8. (multiple $project) to project in a specific order

Here I used `$project` twice consecutively. First time to put the key-value fields in a specific order (i.e., createdAt, then claimedAt, then submittedAt, then reviewedAt). Second time to get the original field names.


```{python}
db.bounties.aggregate([
    {$match: {"$and": [{season: 2}, {customer_id: '905250069463326740'}]}},
    {$project: {_id: 0, _customer_id: "$customer_id", _title: "$title", _status: "$status", _createdAt: "$createdAt", _claimedAt: "$claimedAt", _submittedAt: "$submittedAt", _reviewedAt: "$reviewedAt"}},
    {$project: {_id: 0, customer_id: "$_customer_id", title: "$_title", status: "$_status", createdAt: "$_createdAt", claimedAt: "$_claimedAt", submittedAt: "$_submittedAt", reviewedAt: "$_reviewedAt"}}
])

```


For more content on data science, R, and Python [find me on Twitter](https://twitter.com/paulapivat).