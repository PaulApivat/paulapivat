---
authors:
- admin
categories: []
date: "2020-11-03T00:00:00Z"
draft: false
featured: false
image:
  caption: ""
  focal_point: ""
lastMod: "2020-11-03T00:00:00Z"
projects: []
subtitle: Frequently Used Python Operations 
summary: Problem solving in Python
tags: []
title: Data Science from Scratch (ch1)
---

**Table of Content:**
- [Part 1](#datascienster_pt1)
- [Part 2](#datascienster_pt2)

## DataScienster_pt1

#### Collections and Comprehensions

[Data Science from Scratch](https://joelgrus.com/2019/05/13/data-science-from-scratch-second-edition/) opens with a narrative motivating example where you, dear reader, are newly hired to lead data science at *DataSciencester*, a social network exclusively for data scientists. 

Joel Grus, the author, explains:

> Throughout the book, we'll be learning about data science concepts by solving problems that you encounter at work. Sometimes we'll look at data explicitly supplied by users, sometimes we'll look at data generated through their interactions with the site, and sometimes we'll even look at data from experiments that we'll design...we'll be building our tools from scratch.

This chapter is meant as a teaser for the rest of the book, but I wanted to revisit this chapter with our [python crash course](https://paulapivat.com/post/dsfs_2/) fresh on our minds to highlight some *frequently* used concepts we can expect to see for the rest of the book.

You are just hired as "VP of Networking" and are tasked with finding out which data scientist is the most well connected in the DataSciencster network, you're giving a data dump 👇. It's a list of users, each with a unique id. 

```python
users = [
    {"id": 0, "name": "Hero"},
    {"id": 1, "name": "Dunn"},
    {"id": 2, "name": "Sue"},
    {"id": 3, "name": "Chi"},
    {"id": 4, "name": "Thor"},
    {"id": 5, "name": "Clive"},
    {"id": 6, "name": "Hicks"},
    {"id": 7, "name": "Devin"},
    {"id": 8, "name": "Kate"},
    {"id": 9, "name": "Klein"}
]
```
Of **note** here is that the `users` variable is a `list` of `dict` (dictionaries). 

Moving along, we also receive "friendship" data. Of **note** here that this is a `list` of `tuples`:

```python
friendship_pairs = [(0,1), (0,2), (1,2), (1,3), (2,3), (3,4),
                    (4,5), (5,6), (5,7), (6,8), (7,8), (8,9)]
```
I had initially (and erroneously) thought of `list`, `dict` and `tuple` as **data types** (like `int64`, `float64`, `string`).

They're rather **collections**, and somewhat unique to Python and more importantly, *informs the way Pythonistas approach and solve problems*. 

You may feel that having "friendship" data in a `list` of `tuple` is not the easiest way to work with data (nor may it be the best way to represent data, but we'll suspend those thoughts for now). Our first task is to convert this `list` of `tuple` into a form that's more workable; the author proposes we turn it into a `dict` where the `keys` are user_ids and the `values` are `list` of friends.

The argument is that its faster to look things up in a `dict` rather than a `list` of `tuple` (where we'd have to iterate over every `tuple`). Here's how we'd do that:

```python
# Initialize the dict with an empty list for each user id
friendships = { user["id"]: [] for user in users }

# Loop over friendship pairs 
# This operation grabs the first, then second integer in each tuple
# It then appends each integer to the newly initialized friendships dict
for i, j in friendship_pairs:
    friendships[i].append(j)
    friendships[j].append(i)
```
We're *initializing* a `dict` (called `friendships`), then looping over `friendship_pairs` to populate `friendships`. This is the outcome:

```python
friendships

{
 0: [1, 2],
 1: [0, 2, 3],
 2: [0, 1, 3],
 3: [1, 2, 4],
 4: [3, 5],
 5: [4, 6, 7],
 6: [5, 8],
 7: [5, 8],
 8: [6, 7, 9],
 9: [8]
}
```
Each `key` in friendships is matched with a `value` that is initially an empty list, which then gets populated as we loop over `friendship_pairs` and systematically append the user_id that is paired together.

To understand how the looping happends and, specifically how each **pair** of user_ids are connected to each other, I created my own mini-toy example. Let's say we're just going to focus on looping through `friendship_pairs` for the user **Hero** whose id is 0. 

```python
# we'll set hero to an empty list
hero = []

# for every friendship_pair, if the first integer is 0, which is Hero's id,
# then append the second integer
for x, y in friendship_pairs:
    if x == 0:
        hero.append(y)
        
# outcome: we can confirm that Hero is connected to  Dunn and Sue
hero # [1,2]
```
The above gave me better intuition for how this works:

```python
for i, j in friendship_pairs:
    friendships[i].append(j)  # Add j as a friend of user i
    friendships[j].append(i)  # Add i as a friend of user j
```

Here are some other questions we may be interested in:

#### What is the total number of connections?

Look at how the problem is solved. What's notable to me is how we first define a function `number_of_friends(user)` that returns the number of friends for a particular user.

Then, `total_connections` is calculated using a **comprehension** (tuple?):

```python
def number_of_friends(user):
    """How many friends does _user_ have?"""
    user_id = user["id"]
    friend_ids = friendships[user_id]
    return len(friend_ids)

total_connections = sum(number_of_friends(user) for user in users)
```
To be clear, the **(tuple) comprehension** is a pattern where a function is applied over a for-loop, in one line:

```python
# (2, 3, 3, 3, 2, 3, 2, 2, 3, 1)
tuple((number_of_friends(user) for user in users))

# you can double check by calling friendships dict and counting the number of friends each user has
friendships

{
 0: [1, 2],
 1: [0, 2, 3],
 2: [0, 1, 3],
 3: [1, 2, 4],
 4: [3, 5],
 5: [4, 6, 7],
 6: [5, 8],
 7: [5, 8],
 8: [6, 7, 9],
 9: [8]
}
```
This pattern of using a one-line for-loop (aka comprehension) will come up often. If we add up all the connections, we get 24 and to find the average, we simply divide by the number of users (10) for 2.4, this part is straight-forward.

#### Can we sort who has most-to-least friends to find the most connected individuals?

To answer this question, again, a **list comprehension** is used. The cool thing is that we re-use functions we had previously created (`number_of_friends(user)`).

```python
# Create a list that loops over users dict, applying a previously defined function
num_friends_by_id = [(user["id"], number_of_friends(user)) for user in users]

# Then sort
num_friends_by_id.sort(                                 # Sort the list
    key=lambda id_and_friends: id_and_friends[1],       # by number friends
    reverse=True)                                       # descending order
```

We have just identified how *central* an individual is to the network, and we can expect to explore **degree centrality** and **networks** more in future chapters, but for the purposes of *this* post, we have identified the central role that **collections** (lists, dictionaries, tuples) as well as **comprehensions** play in Python operations. 

In the next post, we'll examing how friendship connections may or may not overlap with interests.

## DataScienster_pt2










