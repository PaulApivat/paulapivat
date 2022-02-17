---
date: "2022-02-08T00:00:00+01:00"
draft: false
linktitle: Nested JSON to dataframe
menu:
  example_tech:
    parent: Python Tips 
    weight: 1
title: Create DataFrames from Nested JSON data
image:
  caption: ""
  focal_point: ""
toc: true
type: docs
weight: 1
---

## Turning Nested JSON data into dataframes

**Situation**: You've connected to an [API endpoint](https://paulapivat.com/technical_notes/example_tech/python_api_connection/), that is structured as a nested json, here's how to loop through and select certain values into a dataframe for further processing.

In one example, we're looping through a particular endpoint from [Coordinape](https://coordinape.com/). We're going to loop through one level (list of dictionaries) and **append** data to **empty lists**. 

Second example, we'll *conditionally* loop through the *second level* (a dictionary) to grab specific values. As a *bonus*, we'll also use **try-except**(error handling) to handle an `AttributeError`. 

Finally, we'll create the **dataframe from lists**.



```{python}

# empty lists
name_list_24 = []
address_list_24 = []
circle_id_list_24 = []
discord_username_list_24 = []
profile_address_24 = []


# loop through level 1
for dct in df_manifest_24['circle.users'][0]:
    name_list_24.append(dct['name'])
    address_list_24.append(dct['address'])
    circle_id_list_24.append(dct['circle_id'])


# conditionally loop through level 2 
# use try-except
for dct in df_manifest_24['circle.users'][0]:
    try:
        for k, v in dct['profile'].items():
            if k == 'discord_username':
                discord_username_list_24.append(v)
            elif k == 'address':
                profile_address_24.append(v)
            else:
                print("Done.")
    except:
        AttributeError
    pass

# create dataframe from lists
df_24 = pd.DataFrame(list(zip(name_list_24, address_list_24, circle_id_list_24)),
                     columns=['Name', 'Address', 'Circle_Id'])

```

## Loading Nested Data `json_normalize`

**Situation**: Before creating a dataframe as we saw above, sometimes we just need to convert *nested* JSON data.

**Data breakdown**:

- `data` is variable we save json dump coming in from http request.
- flatten a portion of `data`, data['businesses'] and load into dataframe, 'bookstores'
- the 'bookstores' dataframe has many columns, some of which is a *column of arrays*
- one column, `categories` is a column of arrays (deeply nested data)
- now, flatten **that** data (categories) setting parameters to `json_normalize`, including
- data, sep, record_path, meta and meta-prefix


```{python}
# import json_normalize
import pandas as pd
import requests
from pandas.io.json import json_normalize

# use dotenv to hide token info
import os
from dotenv import load_dotenv   
load_dotenv()    

# headers
api_url = os.environ.get("API_URL")
api_key = os.environ.get("API_KEY")
headers = {"Authorization": "Bearer {}".format(api_key)}
params = {"term": "bookstore",
          "location": "San Francisco"}
          
# make API call and extract JSON data
response = requests.get(api_url,
                        headers=headers,
                        params=params)
                        
# data is entire json dump                       
data = response.json()

# data is also nested json
# flatten data and load to dataframe with _ separator
bookstores = json_normalize(data['businesses'], sep="_")
print(list(bookstores))

# print deeply nested data
# within bookstores dataframe, choose "categories" column
print(bookstores.categories.head())

# flatten categories data, bring in business details
df = json_normalize(data['businesses'],
                    sep="_",
                    record_path = "categories",
                    meta = ["name",
                            "alias",
                            "rating",
                            ["coordinates", "latitude"],
                            ["coordinates", "longitude"]],
                    meta_prefix="biz_")





```

For more content on data and web3 [find me on Twitter](https://twitter.com/paulapivat).

