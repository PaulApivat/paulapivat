---
date: "2021-12-22T00:00:00+01:00"
draft: false
linktitle: Show current user
menu:
  example_tech:
    parent: Mongodb
    weight: 2
title: Show current user, db access and role
toc: true
type: docs
weight: 2
---

## Show current user

**Situation**: You want to grab *your* own info as the current user of MongoDB.

Run this command in Mongo Shell.

```{python}
db.name_of_collection.runCommand({connectionStatus: 1})

# once you get user name, you can use this command
db.getUser("UserName")
```



For more content on data [find me on Twitter](https://twitter.com/paulapivat).