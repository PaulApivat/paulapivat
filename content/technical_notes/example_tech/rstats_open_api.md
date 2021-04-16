---
date: "2021-04-16T00:00:00+01:00"
draft: false
linktitle: Open API with R
menu:
  example_tech:
    parent: Rstats Tip
    weight: 2
title: Consume Open API with R
toc: true
type: docs
weight: 2
---

## Reading in JSON data from an Open API

The following example is an [Open API](https://covid19.th-stat.com/api/open/timeline) from the Ministry of Public Health in Thailand. 

The following script consumes the API using the `httr` package, then transforms JSON to dataframe via the `jsonlite` package. 

```
install.packages("httr")
install.packages("jsonlite")
library(httr)
library(jsonlite)

# send a GET request to the Ministry of Public Health Open API
# consume API to receive JSON file
url <- "https://covid19.th-stat.com/api/open/timeline"
resp <- GET(url = url)

# convert JSON file into text
text_json <- content(resp, as = "text", encoding = "UTF-8")

# read text from JSON
jfile <- fromJSON(text_json)

# save as data frame
df <- as.data.frame(jfile)

# view data frame
View(df)

```


