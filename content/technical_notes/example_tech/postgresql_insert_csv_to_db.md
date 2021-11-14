---
date: "2021-11-07T00:00:00+01:00"
draft: false
linktitle: Insert data to database
menu:
  example_tech:
    parent: PostgreSQL
    weight: 2
title: Manually insert data to database
toc: true
type: docs
weight: 2
---

## Delete a table 

**Situation**: If you have a CSV file, you can manually load it into PostgreSQL through the pgAdmin client. This may require creating the data values in Excel first. 

First, you'd have to create a table with the right columns in pgAdmin before inserting data in. 

To create in Excel, you'd need a function to copy the values from the dataframe into a tuple of string values. Data is cut short to keep the example manageable.

```{python}
# In Excel
=CONCATENATE("('",C2,"','",A2,"','",B2,"','",E2,"','",D2,"'),")

=CONCATENATE("('",amount_display,"','",from_address,"','",id,"','",timestamp,"','",to_address,"'),")

# sample tuple format
('0x7a250d5630b4cf539739df2c5dacb4c659f2488d','14897.1883870177','0x59c1349bc6f28a427e78ddb6130ec669c2f39b48','0x0f433138b2a8f2997ef387ffcebec7cd204ab2053c43f8d4a6efaa74eddc0e0c-23','1620159318','Tue, 04 May 2021 20:15:18 GMT'),
```

Once the data is prepped in Excel, you can manully paste into pgAdmin (note: can be error prone with 20,000+ rows). Data is cut short to keep the example manageable.

```{python}
INSERT INTO public.table_to_insert(
        amount_display, from_address, id, timestamp, to_address)
        VALUES
        ('14897.1883870177','0x59c1349bc6f28a427e78ddb6130ec669c2f39b48','0x0f433138b2a8f2997ef387ffcebec7cd204ab2053c43f8d4a6efaa74eddc0e0c-23','1620159318','Tue, 04 May 2021 20:15:18 GMT');
```

Note: this is similar to how it's done using the `sqlalchemy` library in python.

```{python}
# Sending Multiple Parameters
with engine.connect() as conn:
    conn.execute(
        text("INSERT INTO some_table (x, y) VALUES (:x, :y)"),
        [{"x": 11, "y": 12}, {"x": 13, "y": 14}]
    )
    conn.commit()
```

## Demo

**Situation**: You have to `CREATE TABLE` *before* you `INSERT INTO`. Here's a full example of creating a table in Postgres. The code is truncated to save time.

Incidentally, you have to delete the table if you mistakenly created it in Postgres (use SQL commands to *Create* the table.)

NOTE: **PGAdmin** is the client, but this should transfer across client.

```{python}
# delete table (if needed)
DROP TABLE bankless_snapshot_header_1;

# create table before insert in postgres
CREATE TABLE IF NOT EXISTS bankless_snapshot_header_1 (
	id SERIAL,
	proposal_id VARCHAR(100),
	title VARCHAR(2000),
	start_date BIGINT,
	end_date BIGINT,
	PRIMARY KEY (proposal_id)
)

# insert data (copied from csv) to postgres
INSERT INTO bankless_snapshot_header_1(
	id, proposal_id, title, start_date, end_date)
	VALUES
	('0','QmdoixPMMT76vSt6ewkE87JZJywS1piYsGC3nJJpcrPXKS','Approve the Bankless DAO Genesis¬†Proposal?','1620154800','1620414000'),
('1','QmbCCAH3WbAFJS4FAUTvpWGmtgbaeMh1zoKgocdt3ZJmdr','What charity should CMS Holdings donate 100k towards? ','1620327600','1620673200'),
('2','QmYvsZ7AU2XyhpBL8g4QRQbLRX6uU3t5CHNdFQbs5r7ypJ','Badge Distribution for Second Airdrop','1620759600','1621018800'),
('3','QmQX2DQcDTZzCpM6DTVNJutQJwWXtxJDTMpBoFjbnaM9i2','Reward Season 0 Active Members ','1623196800','1623456000'),
('4','QmXrfAHMoRcu5Vy3DsRTfokqLBTEKR6tqKVecLvkgw5NZf','Bankless DAO Season 1 ','1623985200','1624590000');
```

## Demo 2

**Context**: For the Snapshot data pipeline, I had to create two pipes - one for proposals and one for votes. This is the process for votes, it's similar, but there are differences:

I initially set `FOREIGN KEY (proposal_id)`, but got a syntax error, there's a [specific way to set up foreign key constraints first](https://www.postgresqltutorial.com/postgresql-foreign-key/) before explicitly define foreign key during `CREATE TABLE` events.

Also, some rows at non-explicit null values (''), so I had to manually go line-by-line to set to `NULL`.

```{python}
# Create Table in Postgresql

CREATE TABLE IF NOT EXISTS stg_bankless_snapshot_1(
	id SERIAL,
	vote_id VARCHAR(100),
	voter VARCHAR(100),
	created BIGINT,
	choice REAL,
	__typename VARCHAR(20),
	proposal_id VARCHAR(100)
)

# Insert data
INSERT INTO stg_bankless_snapshot_1(
	id, vote_id, voter, created, choice, __typename, proposal_id)
	VALUES
('0','QmQFvHkah7w2qAcY4iECn6THDbaypto8JVF5G6YQaneZRV','0xD00dF71434Cf40b2CDb65ff73bD9789933adA44A','1620413879','1','Vote','QmdoixPMMT76vSt6ewkE87JZJywS1piYsGC3nJJpcrPXKS'),
('1','QmSS2x2xBRwTigXR5vucVp75FqCP5ns3CLYK3dLgNQonkC','0x910176D294AFA2cD017928cA92a0bf5a01152194','1620413347','1','Vote','QmdoixPMMT76vSt6ewkE87JZJywS1piYsGC3nJJpcrPXKS'),
('2','QmSa7QFD3vsV6bhsfSKGW1tUtQyJk3umMTgVkFS1H8fnXJ','0x37bf9E28E099335DCec53a8b7FadeFDE6DbF108d','1620410370','1','Vote','QmdoixPMMT76vSt6ewkE87JZJywS1piYsGC3nJJpcrPXKS'),

```

For more content on Data and DAOs [find me on Twitter](https://twitter.com/paulapivat).