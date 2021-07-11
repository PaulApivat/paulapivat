---
date: "2021-07-11T00:00:00+01:00"
draft: false
linktitle: Faker in Python
menu:
  example_tech:
    parent: Python Tips 
    weight: 1
title: Using Faker to simulate fake data with Python
image:
  caption: ""
  focal_point: ""
toc: true
type: docs
weight: 1
---

## Creating mock data in Python with the Faker library

If you need to simulate mock data, the `Faker` [library](https://github.com/joke2k/faker) is a great resource. Shout out to [@joke2k](https://twitter.com/joke2k) for maintaining this project. 

The full code for a recent project is below, with breakdown of each section to follow. Project context: Utilizing MongoDB as our NoSQL database. I've created a basic document (json) and now need to replicate multiple documents to simulate data once it's populated with data from the frontend (or a Bot for this project specifically).

```
from json import dumps
from faker import Faker
import collections

database = []
filename = 'testing_bounty'
length = 5
fake = Faker()

# fake.word(ext_word_list=)
random_currencies = ['BANK', 'ETH', 'BTC']

random_guilds = ["Marketing Guild", "Treasury Guild",
                 "Developer's Guild", "Analytics Guild", "Writer's Guild"]

discord_handle = ["@bob#8888", "@alice#1234",
                  "@carol#5555", "@delta#2222", "@lambda#3333"]

bounty_status = ['Open', 'Draft', 'In-Progress',
                 'In-Review', 'Completed', 'Deleted']

skills = ["writing",
          "design",
          "software development",
          "strategic planning",
          "data analysis",
          "grant writing",
          "proposal development",
          "team building",
          "marketing"]

for x in range(length):
    database.append(collections.OrderedDict([
        ('season', fake.random_int(0, 10)),
        ('bounty', fake.sentence()),
        ('bountyDescription', fake.sentence()),
        ('doneCriteria', fake.sentence()),
        ('bountyReward', collections.OrderedDict([
            ('currency', fake.word(ext_word_list=random_currencies)),
            ('amount', fake.random_int(0, 50000))
        ])),
        # list of dictionaries
        ('applicableGuilds', [collections.OrderedDict(
            [('guildName', fake.word(ext_word_list=random_guilds))]), collections.OrderedDict([('guildName', fake.word(ext_word_list=random_guilds))])]),
        ('bountyCreatedBy', collections.OrderedDict([
            ('isDaoMember', fake.pybool()),
            ('guildName', fake.word(ext_word_list=random_guilds)),
            ('discordHandle', fake.word(ext_word_list=discord_handle)),
            ('publicAddress', "0x2d94aa3e47d9d5024503ca8" + fake.pystr())
        ])),
        ('bountyCreatedAt', fake.date_between(start_date='today', end_date='+3m')),
        ('bountyDueAt', fake.date_between(start_date='today', end_date='+1y')),
        ('bountyActivatedAt', fake.date_between(
            start_date='today', end_date='+6m')),
        ('bountyClaimedBy', collections.OrderedDict([
            ('guildName', fake.word(ext_word_list=random_guilds)),
            ('discordHandle', fake.word(ext_word_list=discord_handle)),
            ('publicAddress', "0x2d94aa3e47d9d5024503ca8" + fake.pystr())
        ])),
        ('bountyClaimedAt', fake.date_between(start_date='today', end_date='+4m')),
        ('bountySubmittedBy', fake.word(ext_word_list=random_guilds)),
        ('bountySubmittedAt', fake.date_between(
            start_date='today', end_date='+11m')),
        ('bountySubmissionLink', "www." + fake.safe_domain_name()),
        # list of dictionaries
        ('bountyStatus', [collections.OrderedDict([
            ('status', fake.word(ext_word_list=bounty_status)),
            ('bountyStatusTime', fake.date_between(
                start_date='today', end_date='+1y'))
        ])]),
        ('bountyHash', fake.md5(raw_output=False)),
        # list of words
        ('skillsRequired', [fake.word(ext_word_list=skills),
                            fake.word(ext_word_list=skills)])
    ]))

with open('%s.json' % filename, 'w') as output:
    # turns date_between into string, circumvent json serialization
    output.write(dumps(database, indent=4, sort_keys=False, default=str))
print("Done.")
```
Here's a breakdown, section-by-section:

Since we'll be creating mock json data, the `json` built-in library is imported (specifically `dumps`), the `Faker` library is the main tool and we'll also be using `OrderedDict` from `collections` to create document objects. 

Database is set to an empty array which will store the json object (`OrderedDict`). We'll save the file that we eventually write as `testing_bounty` and keep it a short length (5), while we're still testing. 

Finally, we initialize the `Faker` library by calling the Faker function and setting to `fake`.

```
from json import dumps
from faker import Faker
import collections

database = []
filename = 'testing_bounty'
length = 5
fake = Faker()   <--- important
```

This next section save a list of words that pertinent to our project in lists. As we create mock json objects, for certain fields, we'll want to populate from a sample of **keywords** that's important for our project.

Without these keywords, we'll need to rely on random words/sentences that comes with the `Faker` library and that may not always be appropriate for our context. 

```
# fake.word(ext_word_list=)
random_currencies = ['BANK', 'ETH', 'BTC']

random_guilds = ["Marketing Guild", "Treasury Guild",
                 "Developer's Guild", "Analytics Guild", "Writer's Guild"]

discord_handle = ["@bob#8888", "@alice#1234",
                  "@carol#5555", "@delta#2222", "@lambda#3333"]

bounty_status = ['Open', 'Draft', 'In-Progress',
                 'In-Review', 'Completed', 'Deleted']

skills = ["writing",
          "design",
          "software development",
          "strategic planning",
          "data analysis",
          "grant writing",
          "proposal development",
          "team building",
          "marketing"]
```

This next section is where we create a number of json objects, specified by length. Everything is pushed into an `collections.OrderedDict` and in some cases, we have nested data. 

Most of the fields are typical `key-value` pairs. There are variations in the `values`, including:

1. strings (`fake.sentence()`, `fake.word()`, `fake.pystr()`); you can sample from list of words created above
2. integers (`fake.random_int()`); you can randomize within a range of integers
3. dates (`fake.date_between`)
4. boolean (`fake.pybool`)
3. dictionaries / json object (`collections.OrderedDict`)
4. list of dictionaries, enclose within `[]`
5. list of strings

```

for x in range(length):
    database.append(collections.OrderedDict([
        ('season', fake.random_int(0, 10)),
        ('bounty', fake.sentence()),
        ('bountyDescription', fake.sentence()),
        ('doneCriteria', fake.sentence()),
        ('bountyReward', collections.OrderedDict([
            ('currency', fake.word(ext_word_list=random_currencies)),
            ('amount', fake.random_int(0, 50000))
        ])),
        # list of dictionaries
        ('applicableGuilds', [collections.OrderedDict(
            [('guildName', fake.word(ext_word_list=random_guilds))]), collections.OrderedDict([('guildName', fake.word(ext_word_list=random_guilds))])]),
        ('bountyCreatedBy', collections.OrderedDict([
            ('isDaoMember', fake.pybool()),
            ('guildName', fake.word(ext_word_list=random_guilds)),
            ('discordHandle', fake.word(ext_word_list=discord_handle)),
            ('publicAddress', "0x2d94aa3e47d9d5024503ca8" + fake.pystr())
        ])),
        ('bountyCreatedAt', fake.date_between(start_date='today', end_date='+3m')),
        ('bountyDueAt', fake.date_between(start_date='today', end_date='+1y')),
        ('bountyActivatedAt', fake.date_between(
            start_date='today', end_date='+6m')),
        ('bountyClaimedBy', collections.OrderedDict([
            ('guildName', fake.word(ext_word_list=random_guilds)),
            ('discordHandle', fake.word(ext_word_list=discord_handle)),
            ('publicAddress', "0x2d94aa3e47d9d5024503ca8" + fake.pystr())
        ])),
        ('bountyClaimedAt', fake.date_between(start_date='today', end_date='+4m')),
        ('bountySubmittedBy', fake.word(ext_word_list=random_guilds)),
        ('bountySubmittedAt', fake.date_between(
            start_date='today', end_date='+11m')),
        ('bountySubmissionLink', "www." + fake.safe_domain_name()),
        # list of dictionaries
        ('bountyStatus', [collections.OrderedDict([
            ('status', fake.word(ext_word_list=bounty_status)),
            ('bountyStatusTime', fake.date_between(
                start_date='today', end_date='+1y'))
        ])]),
        ('bountyHash', fake.md5(raw_output=False)),
        # list of words
        ('skillsRequired', [fake.word(ext_word_list=skills),
                            fake.word(ext_word_list=skills)])
    ]))
```

Once we run our script, we'll want to save and serialize the `OrderedDict` in a json file. We also want to enclose our dates in strings to avoid getting a `json serialization error`; this is done by setting the parameters of `output.write()` to `default=str`.

```
with open('%s.json' % filename, 'w') as output:
    # turns date_between into string, circumvent json serialization
    output.write(dumps(database, indent=4, sort_keys=False, default=str))
print("Done.")
```

