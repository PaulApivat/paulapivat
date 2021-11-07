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


For more content on Data and DAOs [find me on Twitter](https://twitter.com/paulapivat).