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

> Statement 1: What is the probability of the event "both children are girls" (B) conditional on the event "the older child is a girl" (G)?

The probability for statement one is roughly 50% or (1/2).

> Statement 2: What is the probability of the event "both children are girls" (B) conditional on the event "at least one of the children is a girl" (L)?

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








## Bayes_Theorem





For more content on data science, machine learning, R, Python, SQL and more, [find me on Twitter](https://twitter.com/paulapivat).