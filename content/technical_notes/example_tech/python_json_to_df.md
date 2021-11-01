---
date: "2021-11-01T00:00:00+01:00"
draft: false
linktitle: JSON to Dataframes with Pandas
menu:
  example_tech:
    parent: Python Tips 
    weight: 1
title: Using Pandas to Convert JSON to Dataframes
image:
  caption: ""
  focal_point: ""
toc: true
type: docs
weight: 1
---

## Using Pandas to Convert JSON to Dataframes

**Situation**: We are pulling in JSON data and need to convert it to a dataframe to load to a relational database or for further analysis.

It's likely the JSON is nested with at least two levels. Here the `requests` library makes an http request to an API endpoint. A function `run_query(q)` is written to return the request in JSON data.

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

```

Then we need to unnest the JSON into a list of dictionaries before converting into dataframe. THe flow is JSON -> get Items -> turn into List -> dig down into List of Dictionaries -> convert to dataframe:

```{python}
# returns JSON
result = run_query(query)

# get Items
result_items = result.items()
# turn into List
result_list = list(result_items)
# dig down into List of Dictionaries (2 levels)
lst_of_dict = result_list[0][1].get('transferBanks')
# convert to data frame
df = pd.json_normalize(lst_of_dict)
```

For more content on data science, R, and Python [find me on Twitter](https://twitter.com/paulapivat).