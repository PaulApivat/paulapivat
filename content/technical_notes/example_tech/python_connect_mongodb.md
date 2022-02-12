---
date: "2021-12-20T00:00:00+01:00"
draft: false
linktitle: Connect to MongoDB
menu:
  example_tech:
    parent: Python Tips 
    weight: 1
title: Establishing a Mongo database connection with pymongo
image:
  caption: ""
  focal_point: ""
toc: true
type: docs
weight: 1
---

## Establish connection to MongoDB

**Situation**: Querying data in a NoSQL database (mongo) is fine, but for many data analysis tasks, it may be friendlier to operate in a `Python` and `Pandas` environment. 

We will make a database connection, query the the data as a `json` dump and access it as a list of dictionaries. Each mongo collection being largely thought of as a python dictionary. The below example comes from the Bounty Board project i'm working on. 

**Note**: We will employ the **python-dotenv** method for storing environment variables and measure to have `pymongo` previously installed. 

Here are some external reference for accomplishing this task:
- [PyMongo Documentation](https://pymongo.readthedocs.io/en/stable/tutorial.html)
- [Python MongoDB Find in w3schools](https://www.w3schools.com/python/python_mongodb_find.asp)
- [Using Mongo Databases in Python](https://towardsdatascience.com/using-mongo-databases-in-python-e93bc3b6ff5f)
- [Fetch data from MongoDB using Python](https://www.geeksforgeeks.org/how-to-fetch-data-from-mongodb-using-python/)
- [Turn pymongo's find() return a list](https://stackoverflow.com/questions/13210730/how-to-make-pymongos-find-return-a-list)

```{python}
import os
from dotenv import load_dotenv  # for python-dotenv method
load_dotenv()  # for python-dotenv method


def get_database():
    """Description
    import pymongo
    establish connection to mongo client and return
    
    Args: none.
    Return: client connection
    Notes/Raises:
    """
    from pymongo import MongoClient
    import pymongo
    connection_string = os.environ.get('CONNECTION_STRING')
    client = MongoClient(connection_string)
    return client


def get_collection():
    """Description:
    Go through bountyboard database and bounties collection
    
    Args: none.
    Return: all bounties
    Notes/Raises:
    """
    dbname = get_database()
    bb = dbname['bountyboard']
    col = bb['bounties']
    return col


results = list(get_collection().find())
print(type(results))

# if __name__ == "__main__":
#    dbname = get_database()
#    print(dbname)
```


For more content on data and web3 [find me on Twitter](https://twitter.com/paulapivat).

