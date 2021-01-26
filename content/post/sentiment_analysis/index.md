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
subtitle: Sentiment Analysis of Facebook Posts Using Python and R
summary: Using Python and R to read, pre-process, wrangle and visualize data.
tags: ["R", "Data Science", "Facebook Data", "Python", "Sentiment Analysis", "Text Analysis"]
title: How Positive are your Facebook Posts? A Quick Intro to Sentiment Analysis 
---

### Table of contents

- [Overview](#overview)
- [Getting Data](#getting_data)
- [Tokenization](#tokenization)
- [Normalizing Sentences](#normalizing_sentences)
- [Frequency](#frequency)
- [Sentiment Analysis](#sentiment_analysis)
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
from nltk.stem import LancasterStemmer, WordNetLemmatizer
from nltk.corpus import stopwords
from nltk.probability import FreqDist
import re
import unicodedata
import nltk
import json
import inflect
import matplotlib.pyplot as plt
```

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

We'll loop through a list of strings (empty_lst) to tokenize each *sentence* with `nltk.sent_tokenize()`. This yields a list of list, we'll need to flatten it:

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
However, for this post we're normalizing *sentences*, rather than *words*. 

```python
sents = normalize(flat_sent_token)
print("Length of sentences list: ", len(sents))   # 3194
```

## Frequency

You can use the `FreqDist()` function to get the most common sentences or words. After, you could plot a line chart for a visual comparison of the most frequent sentences. 

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

We are taking a **rule-based/lexicon** approach to sentiment analysis because we have a fairly large dataset, but lack labelled data to build a robust training set, thus Machine Learning would **not** be ideal for this task.





## References

1. Hutto, C.J. & Gilbert, E.E. (2014). VADER: A Parsimonious Rule-based Model for Sentiment Analysis of Social Media Text. Eighth International Conference on Weblogs and Social Media (ICWSM-14). Ann Arbor, MI, June 2014.



For more content on data science, machine learning, R, Python, SQL and more, [find me on Twitter](https://twitter.com/paulapivat).