---
date: "2021-11-17T00:00:00+01:00"
draft: false
linktitle: Prevent sensitive info pushed to GitHub
menu:
  example_tech:
    parent: Python Tips 
    weight: 1
title: Setting .env to store sensitive info without pushing to GitHub
image:
  caption: ""
  focal_point: ""
toc: true
type: docs
weight: 1
---

## Overview

FIRST steps for any project are:

1. Go to the directory's root and create `.gitignore` file.
2. Add `.env` (dot env) to `.gitignore` to [hide from GitHub](https://stackoverflow.com/questions/30696930/how-to-hide-env-file-from-github).
3. **note**: Use one `.env` file per project, you can store multiple API Keys, passwords and other sensitive info.
4. Best practice to store `.env` on same root directory as `.gitignore`.

## Setting up dotenv Files

**Use Case**: We have sensitive API Keys and Secret Keys we don't want to put into version control, but we need to access.

The best practice is to create a `.env` file at the root of your project and store the keys in there, and **most importantly**, making sure to include `.env` in a `.gitignore` file so it *does not* get included in versioning. [source](https://dev.to/biplov/handling-passwords-and-secret-keys-using-environment-variables-2ei0)

The challenge is to install the `dotenv` module to access the `load_dotenv` function in order to access the data in the environment variable (`.env`).

First, here's how the structure of the directory *could* look like:

```
.
├── .env
└── settings.py
└── .gitignore
└── project_directory_1
        └── file.py
└── project_directory_2
        └── another_file.py
```

The key things to pay attention here are `.env`, `your_python_script.py` and `.gitignore`.

For the present hypothetical project, here's what needs to be in the `.gitignore` file:

```
#.gitignore

# Environents
.env
```

Here's what needs to be in `.env`. The example here is accessing Twitter API. The first time you're setting this up, I'd recommend using a "fake" CONSUMER_KEY (aka API Key) and CONSUMER_SECRET (API Secret Key) just to test it out and make sure that it doesn't get inadvertently added to version control (**note**: see setting up `.gitignore` above).

```
# you'll put your actual API Keys and Secrete Keys here

TWITTER_CONSUMER_KEY=fakeconsumerkey
TWITTER_CONSUMER_SECRET=fakeconsumersecret
```

Within the `your_python_script.py` file, you'll [have](https://dev.to/biplov/handling-passwords-and-secret-keys-using-environment-variables-2ei0): 

```
from dotenv import load_dotenv   #for python-dotenv method
load_dotenv()                    #for python-dotenv method

import os 

user_name = os.environ.get('USER')
password = os.environ.get('password')

print(user_name, password)

# output

username password
```

From there you'll go into `file.py` and use the dotenv module to access environment variables (sensitive API Keys). 

The example below accesses Twitter API.

```
#file.py

# for python-dotenv method of access environment variables
from twython import Twython
import webbrowser

import os
import dotenv
from dotenv import load_dotenv
load_dotenv()

###### TWITTER API ######

# IMPORTANT: PLUG YOUR KEY AND SECRET IN DIRECTLY (without os.environ.get())
CONSUMER_KEY = os.environ.get("TWITTER_CONSUMER_KEY")         # API Key
CONSUMER_SECRET = os.environ.get("TWITTER_CONSUMER_SECRET")   # API Secret Key
```

Assuming everything is setup properly, you should be able to run the following code within `IPython` and have `CONSUMER_KEY` return a string:

```
import os
import dotenv
from dotenv import load_dotenv

load_dotenv() # True
CONSUMER_KEY = os.environ.get("TWITTER_CONSUMER_KEY")

CONSUMER_KEY  # 'fakeconsumerkey'
```

## Installing dotenv module

This is the challenging part. I had installed `dotenv` but was unable to access it within `settings.py` and `file.py`. 

First, because we're using Python3, anything that involves `pip install` should  be `pip3 install`. 

I had to uninstall, then re-install `dotenv`, and what worked was following this [Stack Overflow answer](https://stackoverflow.com/questions/58943578/i-have-installed-python-dotenv-but-python-cannot-find-it):

```
# update Nov 17, 2021, this works
$ pip3 install -U python-dotenv

# old installation
conda install -c conda-forge python-dotenv
```

Sometimes having both `dotenv` and `python-dotenv` install can cause conflict. In which case, in your virtual environment, try below. Based on [this answer](https://stackoverflow.com/questions/58943578/i-have-installed-python-dotenv-but-python-cannot-find-it).

```
# update Nov 17, 2021 - use python-dotenv method

pip3 uninstall dotenv
pip3 uninstall python-dotenv
pip3 install python-dotenv
```

You'll know you have something working when you run the following code in the command line:

```
dotenv --version   # dotenv, version 0.19.2   (Nov 17, 2021)
```

**NOTE**: Sometimes a virtual environment will have different `python-dotenv` installed:

```
pip3 show python-dotenv
```
