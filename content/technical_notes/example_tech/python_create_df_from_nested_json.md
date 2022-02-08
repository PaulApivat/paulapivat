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


For more content on data and web3 [find me on Twitter](https://twitter.com/paulapivat).

