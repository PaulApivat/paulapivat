---
date: "2021-12-17T00:00:00+01:00"
draft: false
linktitle: Concatenate String
menu:
  example_tech:
    parent: Mongodb
    weight: 2
title: Concatenate String
toc: true
type: docs
weight: 2
---

## Combine string variables to use in Aggregation Pipeline

**Situation**: You have a "first name" and "last name" you want to concatenate to use in a `$project` stage.

Note: We filter for males only, then also limit results to 10 documents. 

```{python}
# match by gender first if want to filter for only male
db.contacts.aggregate([
    {$match: {gender: "male"}},
    {$project: {_id: 0, name: {$concat: ["$name.first", " ", "$name.last"]}, birthdate: {$toDate: "$dob.date"}}},
    {$sort: {birthdate: 1}},
    {$skip: 10},
    {$limit: 10}
]).pretty()
```



For more content on data science, R, and Python [find me on Twitter](https://twitter.com/paulapivat).