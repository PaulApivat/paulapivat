---
date: "2022-02-03T00:00:00+01:00"
draft: false
linktitle: Get unique users 
menu:
  example_tech:
    parent: Mongodb
    weight: 2
title: Get unique users, week-to-week
toc: true
type: docs
weight: 2
---

## Getting Unique Users

**Situation**: You want to query both **total** and **unique** users on a week-to-week basis. Here's an example with *unique creators* of bounties. 

Change the MongoDB code for claimer, submitter and reviewer. 

```{python}
# Total and Unique Bounty Creators by Week
# Jan 1 - Jan 31, 2022
# NOTE: change MongoDB code with claimer, submitter or reviewer data
db.bounties.aggregate([
  {
    $match: {
      $and: [
        { customer_id: "8888888888888888" },
        { createdAt: { $gte: "2022-01-01" } },
        { createdAt: { $lt: "2022-02-01" } },
      ],
    },
  },
  {
    $project: {
      _id: 1,
      createdAt: { $toDate: "$createdAt" },
      "createdBy.discordHandle": 1,
    },
  },
  {
    $group: {
      _id: { week: { $isoWeek: "$createdAt" } },
      num_creators: { $sum: 1 },
      unique_creators: { $addToSet: "$createdBy.discordHandle" },
    },
  },
]);

```

For more content on data science, R, and Python [find me on Twitter](https://twitter.com/paulapivat).