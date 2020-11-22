---
authors:
- admin
categories: []
date: "2020-11-22T00:00:00Z"
draft: false
featured: false
image:
  caption: ""
  focal_point: ""
lastMod: "2020-11-22T00:00:00Z"
projects: []
subtitle: Conditional Probability & Bayes Theorem
summary: Gaining an intuition for probability using Python
tags: ["Python", "Data Science"]
title: Data Science from Scratch (ch6) - Probability
---

### Table of contents

- [Challenge](#challenge)
- [Marginal and Joint Probability](#marginal_and_joint_probabilities)
- [Conditional Probability](#conditional_probability)
- [Bayes' Theorem](#bayes_theorem)


## Overview

## Challenge

The first challenge in this section is distinguishing between **two** conditional probability statements. 

Here's the setup. We have a family with two (unknown) children with two assumptions. First, each child is equally likely to be a boy or a girl. Second, the gender of the second child is *independent* of the gender of the first child.

> Challenge 1: What is the probability of the event "both children are girls" (B) conditional on the event "the older child is a girl" (G)?

The probability for statement one is roughly 50% or (1/2).

> Challenge 2: What is the probability of the event "both children are girls" (B) conditional on the event "at least one of the children is a girl" (L)?

The probability for statement two is roughly 33% or (1/3).

But at first glance, they look similar. 

## Marginal_and_Joint_Probabilities 

The book jumps straight to conditional probabilities, but first, we'll have to look at **marginal** and **joint** probabilities. Then we'll create a **joint probabilities table** and **sum** probabilities to help us figure out the differences. We'll then *resume* with **conditional probabilities**. 

Before anything, we need to realize the situation we have is one of **independence**. The gender of one child is **independent** of a second child. 

The intuition for this scenario will be different from a **dependent** situation. For example, if we draw two cards from a deck (without replacement), the probabilities are different. The probability of drawing one King ♠️ is (4/52) and the probability of drawing a second King ♣️ is now (3/51); the probability of the second event (a second King) is *dependent* on the result of the first draw. 

Ok back to the two unknown children. 

We can say the probability of the first child being either a boy or a girl is 50/50. Moreover, the probability of the second child, which is **independent** of the first, is *also* 50/50. Remember, our first assumption is that *each child is equally likely to be a boy or a girl*.

Let's put these numbers in a table. The (1/2) probabilities shown here are called **marginal** probabilities (note how they're at the margins of the table).

![marginal](./marginal.png)

Since we have two gender (much like two sides of a flipped coin), we can intuitively figure out *all* possible outcomes:

1. first child (Boy), second child (Boy)
2. first child (Boy), second child (Girl)
3. first child (Girl), second child (Boy)
4. first child (Girl), second child (Girl)

There are *4 possible outcomes* so the probability of getting any one of the four outcomes is (1/4). We can actually write these probabilities in the middle of the table, the **joint probabilities**:

![joint](./joint.png)


To recap, the probability of the first child being either boy or girl is 50/50, simple enough. The probability of the second child being either boy or girl is also 50/50. When put in a table, this yielded the **marginal probability**. 

Now we want to know the probability of say, 'first child being a boy and second child being a girl'. This is a **joint probability** because is is the probability that the first child take a specific gender (boy) **AND** the second child take a specific gender (girl).

If two event are **independent**, and in this case they are, their **joint probabilities** are the *product* of the probabilities of **each one happening**.

The probability of the first child being a Boy (1/2) **and** second child being a Girl (1/2); The product of each marginal probability is the joint probability (1/2 * 1/2 = 1/4).

![product_marginal](./product_marginal.png)

This can be repeated for the other three joint probabilities. 

## Conditional_Probability

Now we get into **conditional probability** which is the probability of one event happening (i.e., second child being a Boy or Girl) **given that** or **on conditional that** another event happened (i.e., first child being a Boy).

At this point, it might be a good idea to get familiar with notation.

A joint probability is the product of each individual event happening (assuming they are independent events). For example we might have two individual events:

- P(1st Child = Boy): 1/2
- P(2nd Child = Boy): 1/2

Here is their **joint probability**:
- P(1st Child = Boy, 2nd Child = Boy) =>
- P(1st Child = Boy) * P(2nd Child = Boy) => 
- (1/2 * 1/2 = 1/4)

There is a relationship between **conditional** probabilities and **joint** probabilities. 

Here is their **conditional probability**:
- P(2nd Child = Boy | 1st Child = Boy) =>
- P(1st Child = Boy, 2nd Child = Boy) / P(1st Child = Boy)

Thie works out to: 
- (1/4) / (1/2) = 1/2
or 
- (1/4) * (2/1) = 1/2

In other words, the probability that the second child is a boy, given that the first child is a boy is still 50% (this implies that with respect to **conditional** probability, if the events are **independent** it is not different from a single event). 

Now we're ready to tackle the two challenges posed at the beginning of this post.

> Challenge 1: What is the probability of the event "both children are girls" (B) conditional on the event "the older child is a girl" (G)?

Let's break it down. First we want the probability of the event that "both children are girls". We'll take the product of two events; the probability that the first child is a girl (1/2) and the probability that the second child is a girl (1/2). So for **both** child to be girls, 1/2 * 1/2 = 1/4

- P(1st Child = Girl, 2nd Child = Girl) = 1/4

Second, we want that to be **given that** the "older child is a girl". 

- P(1st Child = Girl) = 1/2

**Conditional probability**: 
- P(1st Child = Girl, 2nd Child = Girl) / P(1st Child = Girl)
- (1/4) / (1/2) = **1/2** or roughly **50%**

Now let's break down the second challenge: 

> Challenge 2: What is the probability of the event "both children are girls" (B) conditional on the event "at least one of the children is a girl" (L)?

Again, we start with "both children are girls":

- P(1st Child = Girl, 2nd Child = Girl) = 1/4

Then, we have "on condition that at least one of the children is a girl". We'll reference a **joint probability table**. We see that when trying to figure out the probability that "at least one of the children is a girl", we rule out the scenario where **both** children are boys. The remaining 3 out of 4 probabilities, fit the condition. 

![at least](./at_least.png)

The probability of at least one children being a girl is:
- (1/4) + (1/4) + (1/4) = 3/4

So:
- P(1st Child = Girl, 2nd Child = Girl) / P("at least one child is a girl")
- (1/4) / (3/4) = (1/4) * (4/3) = **1/3** or roughly **33%**

#### Key Take-away

When two events are **independent**, their **joint probability** is the product of each event:
- P(E,F) = P(E) * P(F)

Their **conditional** probability is the **joint probability** divided by the conditional (i.e., P(F)).
- P(E|F) = P(E,F) / P(F)

And so for our two challenge scenarios, we have:

Challenge 1:
- B = probability that both children are girls
- G = probability that the *older* children is a girl

This can be stated as: P(B|G) = P(B,G) / P(G)

Challenge 2:
- B = probability that both children are girls
- L = probability that *at least one* children is a girl

This can be stated as: P(B|L) = P(B,L) / P(L)

#### Python Code

Now that we have an intuition and have worked out the problem on paper, we can use code to express conditional probability:

```python
import enum, random
class Kid(enum.Enum):
    BOY = 0
    GIRL = 1
    
def random_kid() -> Kid:
    return random.choice([Kid.BOY, Kid.GIRL])
    
both_girls = 0
older_girl = 0
either_girl = 0
random.seed(0)
for _ in range(10000):
    younger = random_kid()
    older = random_kid()
    if older == Kid.GIRL:
        older_girl += 1
    if older == Kid.GIRL and younger == Kid.GIRL:
        both_girls += 1
    if older == Kid.GIRL or younger == Kid.GIRL:
        either_girl += 1
        
print("P(both | older):", both_girls / older_girl)   # 0.5007089325501317
print("P(both | either):", both_girls / either_girl) # 0.3311897106109325
```
We can see that code confirms our intuition. 

We use a `for-loop` and `range(10000)` to randomly simulate 10,000 scenarios. The `random_kid` function randomly picks either a boy or girl (assumption #1). We set the following variables to start a 0, `both_girls` (both children are girls); `older_girl` (first child is a girl); and `either_girl` (at least one child is a girl). 

Then, each of these variables are incremented by 1 through each of the 10,000 loops if it meets certain conditions. After we finish looping, we can call on each of the three variables to see if they match our calculations above:

```python
either_girl #7,464 / 10,000 ~ roughly 75% or 3/4 probability that there is at least one girl
both_girls  #2,472 / 10,000 ~ roughly 25% or 1/4 probability that both children are girls
older_girl  #4,937 / 10,000 ~ roughly 50% or 1/2 probability that the first child is a girl
```


We will look at Bayes Theorem next.

## Bayes_Theorem





For more content on data science, machine learning, R, Python, SQL and more, [find me on Twitter](https://twitter.com/paulapivat).