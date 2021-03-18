---
authors:
- admin
categories: []
date: "2021-03-18T00:00:00Z"
draft: false
featured: false
image:
  caption: ""
  focal_point: ""
lastMod: "2021-03-18T00:00:00Z"
projects: []
subtitle: Using visualization and text mining to learn about Thai dishes
summary: Using R and Python to scrape, pre-process, wrangle and visualize data.
tags: ["Web Scraping", Data Viz", "Text Mining", "RStats", "ggplot2", "Python"]
title: Pad Thai is a Terrible Choice
---

### Table of contents

- [Overview](#overview)
- [Web Scraping](#web_scraping)
- [Data Cleaning](#data_cleaning)
- [Data Visualization](#data_visualization)
- [Text Mining](#text_mining)
- [Exploratory Questions](#exploratory_questions)

### Overview

People need to know they have other choices aside from Pad Thai. In fact Pad Thai is one of 53 individual dishes and there are at least 201 shared dishes in Thai cuisine (source: [wikipedia](https://en.wikipedia.org/wiki/List_of_Thai_dishes)).

This project is an opportunity to build a dataset of Thai dishes by scrapping tables off Wikipedia. We will use Python for web scrapping and R for visualization. Web scrapping is done in `Beautiful Soup` (Python) and pre-processed further with `dplyr` and visualized with `ggplot2`.

Aside from furthering our data skills, we also make an open source [contribution](https://github.com/holtzy/R-graph-gallery/pull/34).

The project repo is [here](https://github.com/PaulApivat/thai_dishes).

### Web_Scraping

#### First Round

We scraped over [300 Thai dishes](https://en.wikipedia.org/wiki/List_of_Thai_dishes). For each dish, we got:

- Thai name
- Thai script
- English name
- Region
- Description

First, we'll use the following libraries/modules:

```python
import requests
from bs4 import BeautifulSoup
import urllib.request
import urllib.parse
import urllib.error
import ssl
import pandas as pd
```

We'll use `requests` to send an HTTP requests to the wikipedia url we need. We'll access network sockets using 'secure sockets layer' (SSL). Then we'll read in the html data to parse it with **Beautiful Soup**.

To being using **Beautiful Soup**, we want to understand the structure of the page (and tables) we want to scrape. We can see that we want the `table` tag, along with class of `wikitable sortable`.

![png](./web_scrap.png)

The main function we'll use from **Beautiful Soup** is `findAll()` and the main three parameters are `th` (Header Cell in HTML table), `tr` (Row in HTML table) and `td` (Standard Data Cell). 

First, we'll save the table headers in a list, which we'll use when creating an empty `dictionary` to store the data we need. 

```python
header = [item.text.rstrip() for item in all_tables[0].findAll('th')]

table = dict([(x, 0) for x in header])
```

Initially, we want to successfully scrape one table, knowing that we'll need to repeat the process for all 16 tables. Therefore we'll use a *nested loop*. Because all tables have 6 columns, we'll want to create 6 empty lists.

We'll scrape through all table rows `tr` and check for 6 cells (which we should have for 6 columns), then we'll *append* the data to each empty list we create.

```python
# loop through all 16 tables
a = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15]

# empty list to store data
a1 = []
a2 = []
a3 = []
a4 = []
a5 = []
a6 = []

# nested loop for looping through all 16 tables, then all tables individually
for i in a:
    for row in all_tables[i].findAll('tr'):
        cells = row.findAll('td')
        if len(cells) == 6:
            a1.append([string for string in cells[0].strings])
            a2.append(cells[1].find(text=True))
            a3.append(cells[2].find(text=True))
            a4.append(cells[3].find(text=True))
            a5.append(cells[4].find(text=True))
            a6.append([string for string in cells[5].strings])
```

You'll note the code for `a1` and `a6` are slightly different. In retrospect, I found that `cells[0].find(text=True)` did **not** yield certain texts, particularly if they were links, therefore a slight adjustment is made. 

The strings tag returns a `NavigableString` type object while text returns a `unicode` object (see [stack overflow](https://stackoverflow.com/questions/25327693/difference-between-string-and-text-beautifulsoup) explanation).

After we've scrapped the data, we'll need to store the data in a `dictionary` before converting to `data frame`:

```python
# create dictionary
table = dict([(x, 0) for x in header])

# append dictionary with corresponding data list
table['Thai name'] = a1
table['Thai script'] = a2
table['English name'] = a3
table['Image'] = a4
table['Region'] = a5
table['Description'] = a6

# turn dict into dataframe
df_table = pd.DataFrame(table)
```

For `a1` and `a6`, we need to do an extra step of joining the strings together, so I've created two additional corresponding columns, `Thai name 2` and `Description2`:

```python
# Need to Flatten Two Columns: 'Thai name' and 'Description'
# Create two new columns
df_table['Thai name 2'] = ""
df_table['Description2'] = ""

# join all words in the list for each of 328 rows and set to thai_dishes['Description2'] column
# automatically flatten the list
df_table['Description2'] = [
    ' '.join(cell) for cell in df_table['Description']]

df_table['Thai name 2'] = [
    ' '.join(cell) for cell in df_table['Thai name']]
```




Exploring and wrangling data revealed issues in the web scraping process

Second pass to rectify all issues

### Data_Cleaning

- Changing column names (snake case)
- Remove newline escape sequence (\n)
- Add/Mutate new columns:
- major_groupings (individual, shared, savory, sweet, drinks)
- minor_groupings (rice, noodles, curries, soups, salads, grilled etc.)
- Edit rows for missing data in Thai_name column: 26, 110, 157, 234-238, 240, 241, 246
- save to "edit_thai_dishes.csv"

- lead to second-pass web scrappings

### Data_Visualization

- Dividing into Individual vs Shared Dishes
- Dendrogram 1: Individual Dishes
- Dendrogram 2: Shared Dishes

### Text_Mining

- Word Frequency
- Term Document Inverse Document Frequency
- Network of Relationships


### Exploratory_Questions

Here are some exploratory questions generated during this project:

1. How might we organized Thai dishes?
2. What is the best way to organized the different dishes?
3. Which raw material(s) are most popular?
4. Which raw materials are most important?
5. Could you learn about Thai food just from the names of the dishes?




For more content on data science, machine learning, R, Python, SQL and more, [find me on Twitter](https://twitter.com/paulapivat).