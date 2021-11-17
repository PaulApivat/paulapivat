---
date: "2021-11-17T00:00:00+01:00"
draft: false
linktitle: Comparison Operators
menu:
  example_tech:
    parent: GraphQL
    weight: 2
title: Comparison Operators with GraphQL
toc: true
type: docs
weight: 2
---

## Comparison Operators with GraphQL 

**Situation**: When querying a GraphQL endpoint, you might need to query from the *latest* timestamp. You'll need a **comparison operator** for that.

Here's are the operators, taken from [this stackoverflow answer](https://stackoverflow.com/questions/45674423/how-to-filter-greater-than-in-graphql).

**note**: You can add these operators to any key, for example if the column name was "created" (i.e., created at 123456789 timestamp), a query might include `created_gt` to mean "timestamp greater than":

```{python}
# sample query
votes(first: 1000, where: {created_gt: 123456789}) {
        id
        vote_id
        voter
        created
        proposal {
                id
        }
}

# Other comparison operators
_gt (greater than)
_lt (less than)
_gte (greater than or equal to)
_lte (less than or equal to)
_in (equal to)
_not_in (not equal to)

```




For more content on data science, R, and Python [find me on Twitter](https://twitter.com/paulapivat).