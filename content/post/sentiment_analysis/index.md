---
authors:
- admin
categories: []
date: "2021-01-26T00:00:00Z"
draft: false
featured: false
image:
  caption: ""
  focal_point: ""
lastMod: "2021-01-26T00:00:00Z"
projects: []
subtitle: Rule-based Sentiment Analysis Using Python and R
summary: Using Python and R to read, pre-process, wrangle and visualize data.
tags: ["R", "Data Science", "Facebook Data", "Python", "Sentiment Analysis", "Text Analysis"]
title: How Positive are Your Facebook Posts? 
---

### Table of contents

- [Overview](#overview)
- [Getting Data](#getting_data)
- [Tokenization](#tokenization)
- [Normalizing Sentences](#normalizing_sentences)
- [Frequency](#frequency)
- [Sentiment Analysis](#sentiment_analysis)
- [Data Transformation](#data_transformation)
- [Visualization](#visualization)
- [References](#references)



## Overview

#### Why Sentiment Analysis?

NLP is subfield of linguistic, computer science and artificial intelligence ([wiki](https://en.wikipedia.org/wiki/Natural_language_processing)), and you could spend years studying it. 

Alternatively, I wanted a quick mini-dive into this domain, to a get a basic intuition for how NLP works. 

The ideal dataset to work is our own social media post as we may feel connected to the data and motivated to see hidden insights. 

#### How well does Facebook know us? 

How much is too much? 

To find out, I download 14 years of posts to apply basic **text analysis** and **pre-processing**. I  used `Python` to read in `json` data downloaded from Facebook. 

We'll perform pre-processing tasks like tokenization, normalization, stemming and lemmatization aided by Python's **Natural Language Toolkit**, `NLTK`. We can get a frequency of words and sentences from all posts. Finally, we'll use the `VADER` module (Hutto & Gilbert, 2014) for rule-based (or lexicon) model of **sentiment analysis**.

Then, we'll transition our work flow to `R` for **data manipulation** and **visualization**. 


## Getting_Data

First, you'll need to download your own Facebook data by following: Setting & Privacy > Setting > Your Facebook Information > Download Your Information > (select) Posts.

In the example below, I named my file `your_posts_1.json`, but you can change this. 
To read in json, we'll use Python's `json` module. I also like getting a feel for the data with `type` and `len`.

```python
import json

# load json into python, assign to 'data'
with open('your_posts_1.json') as file:
    data = json.load(file)

type(data)     # a list
type(data[0])  # first object in the list: a dictionary
len(data)      # my list contains 2166 dictionaries
```

Here are *all* the libraries we use in this post:

```python
import pandas as pd
from nltk.sentiment.vader import SentimentIntensityAnalyzer
from nltk.stem import LancasterStemmer, WordNetLemmatizer      # OPTIONAL (more relevant for individual words)
from nltk.corpus import stopwords
from nltk.probability import FreqDist
import re
import unicodedata
import nltk
import json
import inflect
import matplotlib.pyplot as plt
```

[Natural Language Tookkit](https://www.nltk.org/) This is a popular Python platform for work with human language data. Ideally for us, it has over 50 lexical resources; we'll be using the [Vader Sentiment Lexicon](https://github.com/cjhutto/vaderSentiment), that is *specifically* attuned to sentiments expressed in social media. Ideal for our use case. NLTK will also help with frequency count, stopwards, stemming and lemmatization.

[Regex](https://docs.python.org/3/library/re.html) We'll use regular expressions to remove punctuation.

[Unicode Database](https://docs.python.org/3/library/unicodedata.html) We'll access Unicode Database to remove non-ASCII characters.

[JSON](https://docs.python.org/3/library/json.html) We'll use the **json** module to read in json from Facebook.

[Inflect](https://pypi.org/project/inflect/) This allows us to convert numbers to words.

[Pandas](https://pandas.pydata.org/) This is a powerful data manipulation and data analysis tool that we'll need when we want to save our text data into a data frame and write to csv.

After we have our data (in `json` format), we'll [dig through](https://twitter.com/paulapivat/status/1352893979897909251?s=20) to get the actual **text data** (our posts). 

We'll store this text in a list.

The code for doing this is. **Note**: the `data` key occassionally returns an empty array and we want to skip over those by checking `len(v) > 0`.

```python
# create empty list
empty_lst = []

# multiple nested loops to store all post in empty list
for dct in data:
    for k, v in dct.items():
        if k == 'data':
            if len(v) > 0:
                for k_i, v_i in vee[0].items():  
                    if k_i == 'post':
                        empty_lst.append(v_i)

print("This is the empty list: ", empty_lst)
print("\nLength of list: ", len(empty_lst))
```

We now have a list of strings.

## Tokenization

We'll loop through a list of strings (empty_lst) to tokenize each *sentence* with `nltk.sent_tokenize()`. The text comes as one big blob and we want to either split into individual words or sentences. I think it'll make more sense to try to find the sentiment of each sentence, so we'll tokenize by sentence. 

This yields a list of list, we'll need to flatten it:

```python
# - list of list, len: 1762 (each list contain sentences)
nested_sent_token = [nltk.sent_tokenize(lst) for lst in empty_lst]

# flatten list, len: 3241
flat_sent_token = [item for sublist in nested_sent_token for item in sublist]
print("Flatten sentence token: ", len(flat_sent_token))
```

## Normalizing_Sentences

For context on the functions used in this section, check out this article by Matthew Mayo on [Text Data Preprocessing](https://www.kdnuggets.com/2018/03/text-data-preprocessing-walkthrough-python.html).

First, we `remove_non_ascii(words)` characters including: `#`, `-`, `'` and `?`, among many others. Then we'll `to_lowercase(words)`, `remove_punctuation(words)`, `replace_numbers(words)`, and `remove_stopwords(words)`. 

Examples stopwords are: your, yours, yourself, yourselves, he, him, his, himself etc. 

All this allows us to clean up the text and have each sentence be on equal playing field. 

```python
# Remove Non-ASCII
def remove_non_ascii(words):
    """Remove non-ASCII character from List of tokenized words"""
    new_words = []
    for word in words:
        new_word = unicodedata.normalize('NFKD', word).encode(
            'ascii', 'ignore').decode('utf-8', 'ignore')
        new_words.append(new_word)
    return new_words


# To LowerCase
def to_lowercase(words):
    """Convert all characters to lowercase from List of tokenized words"""
    new_words = []
    for word in words:
        new_word = word.lower()
        new_words.append(new_word)
    return new_words


# Remove Punctuation , then Re-Plot Frequency Graph
def remove_punctuation(words):
    """Remove punctuation from list of tokenized words"""
    new_words = []
    for word in words:
        new_word = re.sub(r'[^\w\s]', '', word)
        if new_word != '':
            new_words.append(new_word)
    return new_words


# Replace Numbers with Textual Representations
def replace_numbers(words):
    """Replace all interger occurrences in list of tokenized words with textual representation"""
    p = inflect.engine()
    new_words = []
    for word in words:
        if word.isdigit():
            new_word = p.number_to_words(word)
            new_words.append(new_word)
        else:
            new_words.append(word)
    return new_words

# Remove Stopwords
def remove_stopwords(words):
    """Remove stop words from list of tokenized words"""
    new_words = []
    for word in words:
        if word not in stopwords.words('english'):
            new_words.append(word)
    return new_words
    
# Combine all functions into Normalize() function
def normalize(words):
    words = remove_non_ascii(words)
    words = to_lowercase(words)
    words = remove_punctuation(words)
    words = replace_numbers(words)
    words = remove_stopwords(words)
    return words
```
For this post we're normalizing *sentences*, rather than *words*. 

```python
sents = normalize(flat_sent_token)
print("Length of sentences list: ", len(sents))   # 3194
```

**NOTE**: I think the process of stemming and lemmatization for makes more sense for words over sentences, so we won't use them here.

## Frequency

You can use the `FreqDist()` function to get the most common sentences. After, you could plot a line chart for a visual comparison of the most frequent sentences. 

Although simple, counting frequencies can yield some [insights](https://twitter.com/paulapivat/status/1353704114467729408?s=20). 

```python
from nltk.probability import FreqDist

# Find frequency of sentence
fdist_sent = FreqDist(sents)
fdist_sent.most_common(10)   

# Plot
fdist_sent.plot(10)
```

## Sentiment_Analysis

We'll use the `Vader` module from `NLTK`. Vader stands for:

> Valence, Aware, Dictionary and sEntiment Reasoner. 

We are taking a **Rule-based/Lexicon** approach to sentiment analysis because we have a fairly large dataset, but lack labelled data to build a robust training set, thus Machine Learning would **not** be ideal for this task.

Two empty lists are created for a specific purpose.

`sentiment` the result of the first for-loop, capturing each sentence and `sent_scores`, which initializes the `nltk.sentiment.vader.SentimentIntensityAnalyzer` and calculates the **polarity_score** of each sentence (i.e., negative, neutral, positive). Because the polarity_score is stored in a dictionary, I'd like to split each key-value pair separately to eventually have a separate column for each: negative, neutral and positive scores. 

`sentiment2` captures the result of the nested for-loop (2nd layer), where it stores each polarity and value in a list of tuples. 


![sentiment_2](./sentiment_2.png)

After the nested for-loop has appended the data for each sentence (`sentiment`) and their respective individual polarity scores (`sentiment2`, negative, neutral, positive), we'll **create data frames** to store these with pandas. Then, we'll write the data frames to **CSV** to transition to R. Note that we set index to false when saving for CSV. The reason is that Python starts counting at 0, while `R` starts at 1, so these minor differences means we're better off re-creating the index as a separate column in `R`. 

**NOTE**: There is likely a more efficient way for what I'm doing here. My hacky solution is to save two CSV files and move the work flow over to R for further data manipulation and visualization. This is primarily a personal preference for handling data frames and visualizations in R, but this *can* be done in pandas and matplotlib. 

```python
# nltk.download('vader_lexicon')

sid = SentimentIntensityAnalyzer()

sentiment = []
sentiment2 = []

for sent in sents:
    sent1 = sent
    sent_scores = sid.polarity_scores(sent1)
    for x, y in sent_scores.items():
        sentiment2.append((x, y))
    sentiment.append((sent1, sent_scores))
    # print(sentiment)

# sentiment
cols = ['sentence', 'numbers']
result = pd.DataFrame(sentiment, columns=cols)
print("First five rows of results: ", result.head())

# sentiment2
cols2 = ['label', 'values']
result2 = pd.DataFrame(sentiment2, columns=cols2)
print("First five rows of results2: ", result2.head())

# save to CSV
result.to_csv('sent_sentiment.csv', index=False)
result2.to_csv('sent_sentiment_2.csv', index=False)
```

## Data_Transformation

**NOTE**: From this point forward, we'll be using `R` and the `tidyverse` for data manipulation and visualization. `RStudio` is generally the IDE of choice here. We'll create an `R Script` to store all our data transformation and visualization process. We should be in the same directory in which the above CSV files were created with pandas. 

We'll load the two CSV files we saved and the `tidyverse` library:

```r
library(tidyverse)

# load data
df <- read_csv("sent_sentiment.csv")       
df2 <- read_csv('sent_sentiment_2.csv')    
```

We'll create another column that matches the index for the first data frame (sent_sentiment.csv). I save it as `df1`, but you could overwrite the original `df` if you wanted. 

```r
# create a unique identifier for each sentence
df1 <- df %>%
    mutate(row = row_number())
```

Then, for the second data frame (sent_sentiment_2.csv), we'll also create another column matching the index, but also use `pivot_wider` from the `tidyr` package. **NOTE**: You'll want to `group_by` label first, then use `mutate` to create a unique identifier. 

We'll then use `pivot_wider` to ensure that all polarity values (negative, neutral, positive) have their own columns. 

By creating a unique identifier using `mutate` and `row_number()`, we'll be able to join (`left_join`) by row.

After, I save the operation to `df3` which allows me to work off a fresh new data frame for visualization.

```r
# long-to-wide for df2
# note: first, group by label; then, create a unique identifier for each label then use pivot_wider

df3 <- df2 %>%
    group_by(label) %>%
    mutate(row = row_number()) %>%
    pivot_wider(names_from = label, values_from = values) %>%
    left_join(df1, by = 'row') %>%
    select(row, sentence, neg:compound, numbers) 
```

## Visualization

First, we'll visualize the positive and negative polarity scores separately, across all 3194 sentences (your numbers will vary). 

![positivity_line](./positivity_line.png)

![negativity_line](./negativity_line.png)

When I sum positive and negative scores to get a ratio, it's about 568:97 or around 5.8x more positive than negative according to the VADER (Valance Aware Dictionary and Sentiment Reasoner). 

The Vader module will take in every sentence that is inputted and assign a valence score from -1 (most negative) to 1 (most positive). We can either classify sentences as a combination of `pos` (positive), `neu` (neutral) and `neg`(negative) or as a `compound` score (i.e., normalized, weighted composite score). For more details, see [vader-sentiment documentation](https://pypi.org/project/vader-sentiment/).

To see both positive and negative scores together (positive = blue, negative = red, neutral = black).

![sentiment2.png](./sentiment2.png)

Finally, we can also use `histograms` to help us see the distribution of negative and positive sentiment among aggregated Facebook posts:

![patch_histo](./patch_histo.png)

#### Summary

I downloaded 14 years worth of Facebook posts to run a rule-based sentiment analysis and visualize the results, using a combination of Python and R. 

I enjoyed using both Python and R for this project and I think we played to their strengths. I found parsing JSON fairly straight-forward with Python, but once we transition to data frames, I was itching to get back to R. 

Because we lacked labelled data, using a lexicon-approach to sentiment analysis made sense. Now that we have a label for valence scores, it's may be possible to take a machine learning approach to predict the valence of future posts. 


## References

1. Hutto, C.J. & Gilbert, E.E. (2014). VADER: A Parsimonious Rule-based Model for Sentiment Analysis of Social Media Text. Eighth International Conference on Weblogs and Social Media (ICWSM-14). Ann Arbor, MI, June 2014.



For more content on data science, machine learning, R, Python, SQL and more, [find me on Twitter](https://twitter.com/paulapivat).