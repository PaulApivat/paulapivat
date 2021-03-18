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