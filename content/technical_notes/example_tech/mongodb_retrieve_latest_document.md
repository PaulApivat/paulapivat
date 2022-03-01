---
date: "2022-03-01T00:00:00+01:00"
draft: false
linktitle: Retrieve latest document
menu:
  example_tech:
    parent: Mongodb
    weight: 2
title: Method to retrieve latest document
toc: true
type: docs
weight: 2
---

## Method 1: Query collection for latest document 

**Situation**: To get the first document in a collection, the default `findOne()` or `find_one()` function works. However, how do we get the *latest* document?

There are several approaches, but I think this one is the simplest:

```{python}
# first get the total number of documents in the collection
db.collection.count()

# Then use skip()
db.collection.find().skip(db.collection.count() - 1).pretty()
```

### Here's the implementation in Python

The example is taken from the [Bounty Board project](https://twitter.com/daobountyboard). 

```{python}
# find latest document to inspect

# client is a dictionary of databases
db = client['bountyboard']

# collections are attributes of databases
bounties_col = db['bounties']


latest_bounties_doc = bounties_col.find().skip(n_bounties - 1)

print(list(latest_bounties_doc)[0].keys())
```

For more content on data [find me on Twitter](https://twitter.com/paulapivat).