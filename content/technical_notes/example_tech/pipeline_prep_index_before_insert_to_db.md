---
date: "2021-11-07T00:00:00+01:00"
draft: false
linktitle: Prep dataframe to insert to database
menu:
  example_tech:
    parent: Pipeline
    weight: 2
title: Prep dataframe to insert to database
toc: true
type: docs
weight: 2
---

## Prep dataframe to insert to database 

**Situation**: When inserting to postgresql with sqlalchemy, the `to_sql()` function works, but we need to make sure we're appending the `id` (primary key) column the right way -- incrementally.

This will involve manipulating the dataframe with by incremeting with the `max_id` before using `reset_index()` to create an additional column using the natural index, then setting the `index=False` parameter.

```{python}
# change column name
# id, graph_id, amount_display, from_address, to_address, tx_timestamp, timestamp_display

# use rename function to change Two column names, set inplace=False to preserve original dataframe column name
df2 = df.rename(columns={'id': 'graph_id',
                         'timestamp': 'tx_timestamp'}, inplace=False)


# reorder dataframe column using list of names
# list of names (in same order as stg_subgraph_bank)
list_of_col_names = ['graph_id', 'amount_display', 'from_address',
                     'to_address', 'tx_timestamp', 'timestamp_display']
df2 = df2.filter(list_of_col_names)

```

This next part is KEY:

```{python}
df2.index += max_id  # increment with max_id
df2 = df2.reset_index()  # reset index to later increment with max_id

df3 = df2.rename(columns={'index': 'id'}, inplace=False)

# only do this step if you've made sure to duplicate a test table in postgresql, then ensure that the dataframe is in the same shape as the postgresql table
df3.to_sql('stg_subgraph_bank_1', con=db, if_exists='append', index=False)

```


For more content on data science, R, and Python [find me on Twitter](https://twitter.com/paulapivat).