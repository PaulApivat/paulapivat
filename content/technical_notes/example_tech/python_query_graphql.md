---
date: "2021-11-02T00:00:00+01:00"
draft: false
linktitle: Querying GraphQL 
menu:
  example_tech:
    parent: Python Tips 
    weight: 1
title: Using Python to query GraphQL
image:
  caption: ""
  focal_point: ""
toc: true
type: docs
weight: 1
---

## Using Python to query GraphQL

**Situation**: We need to query GraphQL, in JSON format, to convert to dataframe. 

There are several ways to go about this. The simplest way is to use the `requests` library to make HTTP requests to the API endpoint, then the `json` library to convert those requests into JSON format:


```{python}
import requests
import json
import pandas as pd

def run_query(q):
    request = requests.post('https://api-endpoint'
                            '',
                            json={'query': query})
    if request.status_code == 200:
        return request.json()
    else:
        raise Exception('Query failed. return code is {}.     {}'.format(
            request.status_code, query))

# basic query first
query = """
{
    transferBanks(first: 1000, orderBy: timestamp, orderDirection: asc, subgraphError: allow) {
    id
    from_address
    to_address
    amount
    amount_display
    timestamp
    timestamp_display
  }
}
"""

# returns JSON
result = run_query(query)
```

An alternative way is to use the `gql` and the `gql.transport.aiohttp` libraries:

```{python}
from gql import gql, Client
from gql.transport.aiohttp import AIOHTTPTransport

# Select transport with url endpoint
transport = AIOHTTPTransport(
    url="https://api.studio.thegraph.com/query/1121/bankv1/v0.0.5")

# create GraphQL client using defined transport
client = Client(transport=transport, fetch_schema_from_transport=True)

# GraphQL query
query = gql("""
{
    transferBanks(first: 1000, where: {timestamp_gte: "1635403557"}, orderBy: timestamp, orderDirection: asc, subgraphError: allow) {
    id
    from_address
    to_address
    amount
    amount_display
    timestamp
    timestamp_display
    }
}
""")

# run query on transport
result = client.execute(query)
print(result)
```

For more content on data science, R, and Python [find me on Twitter](https://twitter.com/paulapivat).