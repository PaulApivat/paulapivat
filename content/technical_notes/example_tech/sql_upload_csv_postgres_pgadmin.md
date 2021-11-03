---
date: "2021-11-03T00:00:00+01:00"
draft: false
linktitle: Upload CSV to Postgres with pgAdmin
menu:
  example_tech:
    parent: SQL
    weight: 2
title: Use pgAdmin to upload CSV to Postgres 
toc: true
type: docs
weight: 2
---

## Use pgAdmin to upload CSV to Postgres (Quick & Dirty)

**Situation**: There are many ways to upload CSV into Postgres. This is the relatively quick and dirty way. This represents an infrequent step where we happen to be loading a 20,000+ rows as a one-time event with subsequent smaller, more regular, events.


**Context**: The example below is part of a larger process of querying GraphQL in JSON and converting it to Pandas dataframe before getting it into Postgres. Here we are using a mixture of Excel and pgAdmin (Postgres client) to get the job done.

*Note*: pgAdmin happens to be the Postgresql-client I'm using, but any client could work. 

**Pre-requisite Steps**:
1. Create a database table in pgAdmin. Ideally, the columns are defined and consistent with the CSV data that's about to be uploaded.
1a. Assuming a table has already been created, we will be using the `INSERT` statement, otherwise, it would be a `CREATE TABLE`.

2. To `INSERT` table, you'll right click on a table (here we're using, 'stg_subgraph_bank'), select Scripts...,then `INSERT Scripts`. 

The following or close variation should appear. Here we are *inserting into* the `stg_subgraph_bank` table. In this example, there are 6 columns: `to_address`, `amount_display`, `from_address`, `graph_id`, `tx_timestamp` and `timestamp_display`. 

3. The `TRUNCATE TABLE` is to remove existing data before inserting new data (if needed).

```{python}
TRUNCATE TABLE public.stg_subgraph_bank;

INSERT INTO public.stg_subgraph_bank(
	to_address, amount_display, from_address, graph_id, tx_timestamp, timestamp_display)
	VALUES (?, ?, ?, ?, ?, ?);

```

Then, we're turning to Excel to prepare the data that will ultimately replace "VALUE (?, ?, ?, ?, ?, ?)". 

There is a `CONCATENATE` function in Excel that converts data from rows/columns (CSV format) into parentheses of string values. 

```{python}
=CONCATENATE("('",C2,"','",A2,"',",B2,"','",E2,"','",D2,"'),")
```

After that's been created in Excel, we're copying and pasting all 20,000+ rows (or however many) back into pgAdmin.

Here's a truncated version of the 20,000+ rows of data with the `TRUNCATE` command to remove existing data before inserting new data:

```{python}
TRUNCATE TABLE public.stg_subgraph_bank;

INSERT INTO public.stg_subgraph_bank(
	to_address, amount_display, from_address, graph_id, tx_timestamp, timestamp_display)
	VALUES 
	('0x7a250d5630b4cf539739df2c5dacb4c659f2488d','14897.1883870177','0x59c1349bc6f28a427e78ddb6130ec669c2f39b48','0x0f433138b2a8f2997ef387ffcebec7cd204ab2053c43f8d4a6efaa74eddc0e0c-23','1620159318','Tue, 04 May 2021 20:15:18 GMT'),
('0x156d3129b2fd634d5b0817132401aa68b0126098','14897.1883870177','0x7a250d5630b4cf539739df2c5dacb4c659f2488d','0x0f433138b2a8f2997ef387ffcebec7cd204ab2053c43f8d4a6efaa74eddc0e0c-27','1620159318','Tue, 04 May 2021 20:15:18 GMT'),
('0x11ebc944350df20940fb10dd8782d654d6aad8c6','37422.0374220399','0x9d1f1847582261be41f5a54e8b60cad21400c74f','0x355666cd33644fd05b36a54e4ddcd14190a71eea08a291731b6cd9ec8950a199-387','1620159318','Tue, 04 May 2021 20:15:18 GMT'),
('0x5e7a1573620e0df38e41dd302f68d7d8e5b99bba','3231.14250158999','0x9d1f1847582261be41f5a54e8b60cad21400c74f','0x98f688d6adcdbb1a395b21c8f30b81ef0da8454d863e6d6f9a03305c082bae82-263','1620159320','Tue, 04 May 2021 20:15:20 GMT'),
('0x2db3c0f42022fdc8dfe70036fee85e48a24b88af','3949.39809822999','0xfe8cac7dc7ac38da9ba540eb4d1797d0417dcc41','0xc9e209771502f73334340eeea2b943f98d9663a9b1eb4370d23f34a3c860c007-106','1620159320','Tue, 04 May 2021 20:15:20 GMT');
```

Then run the query and the new data should populate the table. 


For more content on data science, R, and Python [find me on Twitter](https://twitter.com/paulapivat).