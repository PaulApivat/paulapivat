---
date: "2021-12-20T00:00:00+01:00"
draft: false
linktitle: Connect to API
menu:
  example_tech:
    parent: Python Tips 
    weight: 1
title: Establishing an API connection with the requests library
image:
  caption: ""
  focal_point: ""
toc: true
type: docs
weight: 1
---

## API Connection

**Situation**: You need to use `requests` to consume an API, but with authorization parameters.

**Note**: This pattern can be used across project, I used this to consume **Coordinape** API data. You'll want to set up a `dot env` file to contain any secret API or authorization keys.

Referencing this [stack overflow](https://stackoverflow.com/questions/19069701/python-requests-library-how-to-pass-authorization-header-with-single-token).

```{python}
# ---- generic pattern

# Request
REQ = 'https://api.site.com/something/else/else'

# Token (stored in .env)
AUTH_TOKEN = 'Bearer 8888|fliuzabuvdgfnsuczkncsq12454632'

# Use os library to pull Token from .env
auth_token = os.environ.get('AUTH_TOKEN')

# Create Header parameter
HEADER = {'Authorization': f'{auth_token}'}

# Putting it all together
response = requests.get(REQ, headers=HEADER)

# Print result
print(response.json())
```

Here's a real example:

```{python}
# request library to establish connection
import requests
from pprint import pprint

# os library used with python-dotenv to grab AUTH_TOKEN from .env
import os

# using python-dotenv method
from dotenv import load_dotenv
load_dotenv()

# authorization & header
auth_token = os.environ.get('AUTH_TOKEN')
HEADER = {'Authorization': f'{auth_token}'}

# ----- Data Endpoints

# Coordinape Round -- OG Bankless(June): Circle id 24
response24 = requests.get(
    'https://api.coordinape.com/api/v2/token-gifts?circle_id=24', headers=HEADER)
pprint(response24.json())
```
