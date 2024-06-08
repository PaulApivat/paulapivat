---
authors:
- admin
categories: []
date: "2024-06-08T00:00:00Z"
draft: false
featured: false
image:
  caption: ""
  focal_point: ""
lastMod: "2024-06-08T00:00:00Z"
projects: []
subtitle: Modeling data to build pipelines
summary: Modeling, then pulling, API data
tags: ["Python", "API", "data modeling", "pipelines", "coingecko"]
title: Exploring CoinGecko's API
---

From a data analyst perspective, one major difference between Web2 and Web3 is the existence of "public" data infrastructure. In crypto, analysts are familiar with [Dune Analytics](https://dune.com/) or [FlipSide Crypto](https://flipsidecrypto.xyz/) among [other data providers](https://www.primodata.org/blockchain-data). Although many of these are private companies, the nature of **public blockchains** make such data more accessible than what you'll find on Google's BigQuery, for example. 

Teams of data engineers, database and backend specialists have done the heavy lifting so analysts can simply "use SQL" right in the browser. This makes the [data workflow in crypto](https://read.cryptodatabytes.com/p/2022-guide-to-web3-data-thinking) unique (and arguably better).

Nevertheless, if you work for a startup that happens to be building data-intensive applications, you might have to pull data from many sources. This post will explore [CoinGecko's API (demo tier)](https://www.coingecko.com/en/api) and describe a process of exploring the data, building an initial model as well as laying the foundation for building data pipelines. 

> Analysts are like restaurant patrons, with forks and knives out, ready to dine. This post will guide you to the kitchen to see how that food is prepared. 

## Tools

My setup, not strictly required to follow along, is as follows:

- Python 3.11.6
- pyenv to manage virtual environments and python versions (follow [Banteg's instructions](https://x.com/bantg/status/1677475400048312320?s=20))

Libraries:
- pandas              2.2.2
- pip                 24.0
- python-dotenv       1.0.1
- requests            2.32.3


## Exploratory Phase: Handling JSON data

API providers will often provide a **Requests - Response** format for visually showing in what format data will come in once a request is made. JSON (javascript object notation) is the norm for text-based structured data delivered over the web. Translated to Python, that becomes **(nested) dictionaries** and **lists**. Depending on the endpoint, data may be nested two or three levels down. There will be in key-value pairs. Values can be strings (text), integers, float or additional lists or dictionaries. 

I find it helpful to make calls to understand what the data looks like visually (in terminal). A simple loop helps iterating through a dictionary (or list):

```
# iterating through a dictionary
for key, value in data.items():
    print("Keys: ", key)
```

But what about **nested** data that **messy**? You may be confronted with lists and/or dictionaries. 

This **helper function** will navigate nested JSON data, print out the key-value pairs and can is generally flexible across a number of cases. It will print out the value as well as keep an index of how many instances of a data structure. For example, I might query a list of coins that's actually a list of dictionaries: 

```
def traverse_json(data, path=[]):
    if isinstance(data, dict):
        print("Working with Dictionary \n")
        for key, value in data.items():
            traverse_json(value, path + [str(key)])  
    elif isinstance(data, list):
        print("Working with List \n")
        for index, item in enumerate(data):
            traverse_json(item, path + [str(index)])  
    else:
        # Terminal value, print path and value
        print(" -> ".join(path) + ":", data)


# Start traversal
traverse_json(data)

```

I'll go through each endpoint I need and make a note of the data structure (List of Dictionaries or a single Dictionary) that it returns. Time spent in the exploratory phase helps get a mental model of the data and begins informing data modeling you might do down the line. 

## Keep API Keys Secret

You don't want to accidentally push your API keys to Github,  create an `.env` (store your API keys here) and `.gitignore` file at the root of your project folder. Finally, use **python-dotenv** to access your environment variables:

```
from dotenv import load_dotenv
import os

load_dotenv()

headers = {
    "accept": "application/json",
    "x-cg-demo-api-key": os.getenv("coingecko_demo_api"),
}

# Coin List (ID Map)
# Response: List of dictionaries

url = "https://api.coingecko.com/api/v3/coins/list"

response = requests.get(url, headers=headers)

print(response.text)
```




I'm always down to talk onchain data, [shoot me a DM](https://twitter.com/paulapivat).