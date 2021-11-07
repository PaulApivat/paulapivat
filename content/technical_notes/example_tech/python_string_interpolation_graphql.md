---
date: "2021-11-07T00:00:00+01:00"
draft: false
linktitle: String Interpolation GraphQL 
menu:
  example_tech:
    parent: Python Tips 
    weight: 1
title: Use string interpolation to query GraphQL 
image:
  caption: ""
  focal_point: ""
toc: true
type: docs
weight: 1
---

## Use Python's string interpolation to query GraphQL 

**Situation**: For this project, I had previously grabbed the latest timestamp in a data table, assigned it to a variable and now want to use it as input for a GraphQL query:



```{python}
# Run separate request to GraphQL endpoint
# use max_tx_timestamp in parameter 'where: {timestamp_gte: max_tx_timestamp}'
# this will return on-chain tx since latest timestamp (i.e., max_tx_timestamp)

variables = {'input': max_tx_timestamp}

query = f"""
{{
  transferBanks(first: 1000, where: {{timestamp_gte:{max_tx_timestamp}}}, orderBy: timestamp, orderDirection: asc, subgraphError: allow) {{
    id
    from_address
    to_address
    amount
    amount_display
    timestamp
    timestamp_display
  }}
}}
"""
```

Then to make sure this *string interpolation* actually works, we need to make a post request to the GraphQL API endpoint, query it, save that query into a data frame. 

(NOTE: This requies toggling back and forth between the database client like pgAdmin and your ipython environment)

```{python}
# note: 'variables' defined above


def run_query(q):
    request = requests.post('https://api.studio.thegraph.com/query/1121/bankv1/v0.0.5'
                            '',
                            json={'query': query, 'variables': variables})
    if request.status_code == 200:
        return request.json()
    else:
        raise Exception('Query failed. return code is {}.     {}'.format(
            request.status_code, query))


result = run_query(query)

# print results
print('Print Bank Subgraph Result - {}'.format(result))
```

For more content on data science, R, and Python [find me on Twitter](https://twitter.com/paulapivat).