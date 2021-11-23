---
date: "2021-11-23T00:00:00+01:00"
draft: false
linktitle: Data Retrieval
menu:
  example_tech:
    parent: Mongodb
    weight: 2
title: Retrieving data in MongoDB
toc: true
type: docs
weight: 2
---

## Using Projection to Limit Set of Fields Retrieved

**Situation**: You want to query a list of discordHandles or customerNames from a collection of documents (list of objects) and you don't want *every* field.


### Returning only one field in a document

```{python}
# list only customer names without their id
db.customers.find({}, {customerName: 1, _id: 0}).pretty()

# list discord handles without the id
db.bounties.find({}, {"createdBy.discordHandle": 1, _id: 0}).pretty()
```

### Using comparison operators to limit number of documents retrieved

```{python}
# list all bounties with reward amount greater than 100
db.bounties.find({"reward.amount": {$gt: 100}}).pretty()

# list all bounties with reward less than 100
db.bounties.find({"reward.amount": {$lt: 100}}).pretty()
```



For more content on data science, R, and Python [find me on Twitter](https://twitter.com/paulapivat).