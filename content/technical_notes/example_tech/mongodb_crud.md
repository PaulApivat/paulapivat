---
date: "2021-11-11T00:00:00+01:00"
draft: false
linktitle: CRUD Operations
menu:
  example_tech:
    parent: Mongodb
    weight: 2
title: CRUD Operations in Mongo (Shell)
toc: true
type: docs
weight: 2
---

## Create, Read, Update, Delete in Mongo (Shell)

**Situation**: This is a summary of basic Mongo operations in shell. These commands can be used in create, seed test databases outside of the production database.

### Basic commands

```{python}
# show available databases
show dbs

# use a database
use db_name

# show collections within database
show collections
```

### Delete an entire collection

```{python}
db.collectionName.drop()
```

### Insert one document into a collection

Create a collection *and* insert one document. Copy a document (i.e., an object or python dictionary) and paste in argument of `.insertOne()`. What's inserted is a single object.

```{python}
db.newCollectionName.insertOne({})
```

**note**: `MongoServerError: _id fields may not contain '$'-prefixed fields: $oid is not valid for storage.` Because mongo shell automatically inserted ids:

Example of two ObjectIds being inserted:
```{python}
{
  acknowledged: true,
  insertedIds: {
    '0': ObjectId("618d1fb2f5975b1a2ed10b91"),
    '1': ObjectId("618d1fb2f5975b1a2ed10b92")
  }
}
```



### Insert many documents into a collection

**note**: to avoid `MongoServerError` with `'$'oid is not valid for storage` error, edit in VSCode before pasting in mongo shell.

Note, parameter is an *array of objects*.

```{python}
db.newCollectionName.insertMany([{}, {}])
```

### Count document(s) inside a collection

```{python}
db.collection.countDocuments()
```

### Find document where integer value > 9000

Using `$gt` (greater than) as an example from the Bounty Board project. 

Alternatives:
- `$lt` less than
- `$gte` greater than or equal to

```{python}
db.bounties2.find({"reward.amount": {$gt: 9000} }).pretty()
```

### Update one document

Updating `season` from 1 to 2:

```{python}
db.bounties2.updateOne({_id: ObjectId("618d2585f5975b1a2ed10b93")}, {$set: {season: 2}})
```

### ReplaceOne instead update

**Note**: This is more tedious than updating just one field; with `replaceOne()` you have to update **all** fields otherwise, they get deleted. Arguably, this makes it more explicit (and safe) way to update.

This replaces key-value pairs in the document with a specific `_id` to only have 2 key-value pairs (erasing all others).

```{python}
db.bounties2.replaceOne({_id: ObjectId("618d1cddf5975b1a2ed10b8f")}, {season: 2, "title": "Implement Changes"})
```
### Print more than just 20 documents

```{python}
bountyboard> db.bounties2.find().forEach((bounties2Data) => {printjson(bounties2Data)})
```

### Return only one field in a document

Here we're returning "title", then "reward.amount".

```{python}
# return title
db.bounties2.find({}, {title: 1, _id: 0}).pretty()

# return reward.amount
db.bounties2.find({}, {"reward.amount": 1, _id: 0}).pretty()
```

### Joining two collections

Note: Join two collections by `customerId`

```{python}
db.bounties2.aggregate([{ $lookup: { from: "customers", localField: "customerId", foreignField: "customerId", as: "customerId" } }])
```

For more content on data science, R, and Python [find me on Twitter](https://twitter.com/paulapivat).