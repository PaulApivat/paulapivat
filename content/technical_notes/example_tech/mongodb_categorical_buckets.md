---
date: "2021-12-17T00:00:00+01:00"
draft: false
linktitle: Categorical Buckets
menu:
  example_tech:
    parent: Mongodb
    weight: 2
title: Create Categorical Buckets
toc: true
type: docs
weight: 2
---

## Create categorical buckets from a continuous variable

**Situation**: You have a continuous variable like "age" and want to create categorical buckets (i.e., ages 10-20, 21-30, 31-40 etc)

Note: We can create minimum ages here

```{python}
# creating age buckets
# group documents into those buckets
# summary statistics

db.contacts.aggregate([
    {$bucket: {groupBy: "$dob.age", boundaries: [18, 30, 40, 50, 60, 120], output: {
        numPersons: { $sum: 1},
        averageAge: {$avg: "$dob.age"}
    }}}
]).pretty()
```

Note: This version auto-generates buckets

```{python}
// buckets auto
// break continuous variable into categorical

db.contacts.aggregate([
 {
     $bucketAuto: {
         groupBy: "$dob.age",
         buckets: 5,
         output: {
            numPersons: { $sum: 1},
            averageAge: {$avg: "$dob.age"}
         }
     }
 }
]).pretty()
```


For more content on data science, R, and Python [find me on Twitter](https://twitter.com/paulapivat).