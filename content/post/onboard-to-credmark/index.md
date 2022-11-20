---
authors:
- admin
categories: []
date: "2022-11-20T00:00:00Z"
draft: false
featured: false
image:
  caption: ""
  focal_point: ""
lastMod: "2022-11-20T00:00:00Z"
projects: []
subtitle: A technical walk-through
summary: Helping data analysts use the Credmark Model Framework
tags: ["Python", "DeFi", "Credmark"]
title: [Mirror] Onboarding to the Credmark Model Framework for Data Analyst
---

**Note**: This is a mirror of a post [originally posted](https://exponent.cx/blog/onboarding-to-the-credmark-model-framework-for-data-analysts) on Exponent's blog. 

*Special thanks to Paul Murphy from Credmark for feedback, Vineet and Kunlun for help getting onboarded to the CMF.*

Many Web3 data analysts are familiar with the SQL-based platforms for accessing on-chain data. Popular providers like Dune Analytics and FlipSideCrypto, among others, come to mind. There are situations where alternative data sources are desirable. 

- You may need to stream data to power your startup’s application. 

- You may need to create predictive models, run portfolio risk simulations, or dig into a DeFi protocol’s equations - many of which may better be accomplished in a Python environment. 

- You may want to cross reference various data sources to ensure you’re getting accurate price data (**note**: Credmark uses a novel approach to solving the problem of getting reliable prices off DEXs. See their [DEX Pricing Model](https://credmark.com/blog/credmarks-dex-pricing-model) and the importance of [decentralized prices](https://credmark.com/blog/decentralized-prices-are-better-than-centralized-prices)). 

For these situations and more, Credmark is a fantastic option that all data teams should consider for their stack. 

Credmark is a blockchain data curator made by developers, for developers. Their target audience has historically been developers and quants. For those coming from ready-to-query SQL-based platforms, there is a bit of a learning curve. 

Beyond simply providing API endpoints, Credmark has created a platform, “[The Credmark Model Framework](https://docs.credmark.com/cmf-model-guide/getting-started/installation-and-setup)”, that allows both consumers of data (i.e., data analysts) and creators of data models to create composable data models, publish them and collect rewards for their work. This is analogous to subgraph developers publishing subgraphs to The Graph’s platform. 

In this article, I’ll walk through a setup of the Credmark Model Framework from the perspective of a Data Analyst who is primarily interested in querying data. The goal is to lower the learning curve allowing more analysts to use this platform.

## Prerequisites

These steps can be worked through on this page, you’ll want to have this page opened in another window. I’ll go over the steps again here in this post, with additional commentary for analysts who may not be familiar with Python. There are some prerequisites needed to prepare your local environment for interacting with the Credmark Model Framework. You’ll want to secure the following:

- Install Python version 3.9+ or Miniconda 4.10+

- A Credmark API Key

- A personal web3 provider URL blockchain node (e.g., an Alchemy HTTPS, under API Key) to run the model 

- VSCode (or a preferred Integrated Development Environment)

- A GitHub account 

- An Anaconda Distribution

- Have access to your terminal from VSCode

- (The assumption is that you’re on a macOS)

Credmark’s Models are stored on a [Github repository](https://github.com/credmark/credmark-models-py) that you’ll need to Fork, then Clone on your local computer. Then you’ll set up a virtual environment. 

For those unfamiliar with Python, googling “python virtual environment” will yield an endless stream of resources. The tl;dr of all this is Credmark’s Model Framework is written in Python, which among other things, allows for model composability - an advanced feature that may not be of immediate concern for the data analyst. However, consequently, analysts who are more interested in querying data have to get comfortable with Python. 

## But what about ‘virtual environments’? 

The Credmark Model Framework comprises Python classes and modules. Users of the framework have to download several popular external data libraries like pandas, numpy, matplotlib among others. To ensure that these libraries (dependencies) don’t get entangled with other projects you may be working on, a virtual environment allows you to create a self-contained environment for when you need to interface with the Credmark Model Framework. 

It is this author’s opinion that the most hassle free way to set up a virtual environment is to use the Anaconda Distribution (but some people may prefer Python venv). 

It is important to create and activate your Python virtual environment *before* you proceed with installing dependencies. You’ll run the next couple commands in your terminal (shell) to list out environments, create one, then make sure you are in that environment.

```
name@your-machine % conda env list
name@your-machine % conda create -name-of-env
name@your-machine % conda activate -name-of-env

```

*Note: After you finish working, you can deactivate your virtual environment:*

```
name@your-machine % conda deactivate
```

After a virtual environment is created and activated, you’ll install dependencies (requirements.txt) and configure your environment which involves *both* a Credmark API Key and an Alchemy (or Infura) URL blockchain node.

```
name@your-machine % pip install -r requirements.txt
```

Next you’ll set up a configuration file at the root of the **credmark-models-py** directory (that you forked and cloned locally, see above), specifically called an “.env” (dot env) file to contain:

```
CREDMARK_API_KEY=your_credmark_api_keys_here CREDMARK_WEB3_PROVIDER_CHAIN_ID_1=your_alchemy_url_blockchain_node_here
```

Storing your API keys in this ‘.env’ file helps prevent it from being uploaded to a public github repository. To double check, you’ll also want to make sure that the ‘.gitignore’ file (also at the root directory) contains ‘.env’ (under the #Environments section). This prevents you from pushing sensitive information to Github, if and when you decide to commit to your own github repository. 

Moreover, sensitive information like API Keys and Alchemy HTTPS url endpoints are in a *separate file* from your Python script that you’ll use to query data. 

If you’re following along the CMF Model Guide documentation, at this point, you’ve completed [Installation & Setup](https://docs.credmark.com/cmf-model-guide/getting-started/installation-and-setup). The rest of the documentation will be of interest if you intend to *build* models, but the assumption for the rest of this post is that as a data analyst, your first task is to query *existing models*. 

First you’ll try out a sample query in your terminal, but in order for it to work, you have to be in the “*models*” directory. Starting at the root directory the path is /credmark-models-py/models. This won’t work unless you’re in the *models* directory. 

Once you’ve changed directory into the models directory, then you can run a sample query in the terminal:

```
$ credmark-dev run price.quote -i '{"base": {"symbol": "WBTC"}}'
```

This should yield the current price of WBTC (you can also substitute it for WETH or ETH or a number of other popular ERC20 tokens). It’s worth noting that symbols are used here for convenience and Credmark maintains a list of verified symbols, however, using the full hexadecimal address will minimize errors. 

To anticipate a possible error message, you may run into the following:

```
zsh: command not found: credmark-dev
```

This means you need to enter another command to set the $PATH to where the command ‘credmark-dev’ is stored by entering the following (note: you may need to customize the following to your version of Python).

```
export PATH=$PATH:/Users/your_name/Library/Python/3.9/bin
```

If this doesn’t work for you, you may have to Google:  “zsh: command not found:” and apply recommendations to your local machine context (i.e., which directory Python is stored on your machine etc). 

Once you get results in the terminal, you can create a Python script to handle both data ingestion and some light transformation.

For this example, we’ll query historical prices. While you can also run this command in the terminal, historical data output can be quite long so handling data and doing light transformations in a python script is more suitable.

Here are the modules we’ll use to run the next few operations.

First, create and save a file. I’ll name mine stableprice.py, but you can name it anything that makes sense to you.

```
import subprocess
import pandas as pd
from pprint import pprint
import json
```

The **subprocess** module allows us to run terminal/shell commands within a Python script. **Pandas** allows us to transform our data; you may want to transform it to dataframe as tabular data is easier to manipulate; **pprint** allows us to print nested dictionaries in a cleaner way and finally, the **json** module allows us to convert string data types to dictionaries.

Continuing on with stableprice.py, i’ll be querying USDC prices for the past 30-days. Below I'm taking a command that could be run in the terminal and breaking it up as a **subprocess** in Python.

We are taking a command that would be one line in terminal:

```
$ credmark-dev run historical.run-model -i
'{"model_slug":"price.quote","model_input":{"base": {"symbol":
"USDC"}},"window":"30 days","interval":"1 day"}'
```

And breaking it up into segments as a python subprocess:

```
usdc = subprocess.run(
    [
        "credmark-dev",
        "run",
        "historical.run-model",
        "-i",
        '{"model_slug":"price.quote","model_input":{"base": {"symbol":
"USDC"}},"window":"30 days","interval":"1 day"}',
    ],
    stdout=subprocess.PIPE,
)

```

We’ve pointed the data at a variable ‘usdc’, which we can further handle. The data that is returned from the Credmark Model Framework are in the bytes, to which we’ll convert to string, then dictionary (see steps below).

Once in dictionary form, we can better decide which key-value pair to move forward with. At this point, the data can be transformed to a dataframe with pandas for further processing. 

```
# bytes data type
output_usdc = usdc.stdout

# bytes to string
output_usdc_str = output_usdc.decode('UTF-8')

# string to dict
output_usdc_dict = json.loads(output_usdc_str)

# list of dict to dataframe
output_usdc_df = pd.DataFrame(output_usdc_dict['output']['series'])
```

In a future post, we’ll explore other Credmark Models as well as build an intuition for looking up the existing library of models. 


If you'd like help with on-chain analysis, please [get in touch](https://twitter.com/paulapivat).