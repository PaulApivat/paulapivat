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

In the previous section, we began examining a toy data set see what kind of Python concepts from the crash course we'd see in action.

What stands out is the use of collections and comprehension. We'll see this trend continue as data is given to us in the form of a list of dict or tuples.

Often time, we're manipulating the data to make it faster and more efficient to iterate through the data. The tool that comes up quite often is using defaultdict to initialize an empty list. Followed by list comprehensions to iterate through data.

Indeed, either we're seeing how the author, specifically, approaches problem or how problems are approached in Python, in general.

What I'm keeping in mind is that there are more than one way to approach data science problems and this is one of them.

With that said, let's pick up where the previous section left off.

#### Friends you may know

We have a sense of the *total number of connections* and a sorting of the *most connected* individuals. Now, we may want to design a "people you may know" suggester. 

Quick recap, here's what the friendship dictionary looks like.

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
Again, the first step is to *iterate* over friends and collect friends' friend. The following function returns a **list comprehension**. Let's examine this function line-by-line to understand how it works. It returns friend_of_a_friend (foaf) id for each of the  individuals' id, then grabing the id of *their* friends. 

We'll break it down in code below this function:

```python
def foaf_ids_bad(user):
    """foaf is short for 'friend of a friend' """
    return [foaf_id
            for friend_id in friendships[user["id"]]
            for foaf_id in friendships[friend_id]]

# Let's take Hero, to see Hero's friends 
# we'll call the first key of the friendships dict
# Hero has two friends with ids 1 and 2
friendships[0]  # [1,2]

# then we'll loop over *each* of the friends
friendships[1]  # [0, 2, 3]
friendships[2]  # [0, 1, 3]

# assert that function works
assert foaf_ids_bad(users[0]) == [0, 2, 3, 0, 1, 3]
```

#### Can we count mutual friends?

To answer this we'll use a `Counter`, which we [learned](https://paulapivat.com/post/dsfs_2/#counters) is a `dict` subclass. Moreover, the function `friends_of_friends(user)`, 

```python
from collections import Counter

def friends_of_friends(user):
    user_id = user["id"]
    return Counter(
        foaf_id
        for friend_id in friendships[user_id]    # for each of my friends,
        for foaf_id in friendships[friend_id]    # find their friends
        if foaf_id != user_id                    # who aren't me
        and foaf_id not in friendships[user_id]  # and aren't my friends
    )

# lets look at Hero
# he has two common friends with Chi 
# Chi is neither Hero nor his direct friends
friends_of_friends(users[0])  # Counter({3: 2})
```

In addition to friendship data, we also have **interest** data. Here we see a `list` of `tuples`, containing a user_id and a string representing a specific of technology.

```python
interests = [
    (0, "Hadoop"), (0, "Big Data"), (0, "HBase"), (0, "Java"),
    (0, "Spark"), (0, "Storm"), (0, "Cassandra"),
    (1, "NoSQL"), (1, "MongoDB"), (1, "Cassandra"), (1, "HBase"),
    (1, "Postgres"), (2, "Python"), (2, "scikit-learn"), (2, "scipy"),
    (2, "numpy"), (2, "statsmodels"), (2, "pandas"), (3, "R"), (3, "Python"),
    (3, "statistics"), (3, "regression"), (3, "probability"),
    (4, "machine learning"), (4, "regression"), (4, "decision trees"),
    (4, "libsvm"), (5, "Python"), (5, "R"), (5, "Java"), (5, "C++"),
    (5, "Haskell"), (5, "programming langauges"), (6, "statistics"),
    (6, "probability"), (6, "mathematics"), (6, "theory"),
    (7, "machine learning"), (7, "scikit-learn"), (7, "Mahout"),
    (7, "neural networks"), (8, "neural networks"), (8, "deep learning"),
    (8, "Big Data"), (8, "artificial intelligence"), (9, "Hadoop"),
    (9, "Java"), (9, "MapReduce"), (9, "Big Data")
    ]

```

First thing we'll do is find users with a specific interest. This is function returns a **list comprehension**. It first split each `tuple` into `user_id` (integer) and `user_interest` (string), then conditionally check if the `string` in the `tuple` matches the input parameter.

```python
def data_scientists_who_like(target_interest):
    """Find the ids of all users who like the target interests."""
    return [user_id
            for user_id, user_interest in interests
            if user_interest == target_interest]
            
# let's see all user_id who likes "statistics"
data_scientists_who_like("statistics")   # [3, 6]

```
We may also want to count the number of times a specific interest comes up. Here's a function for that. We use a basic for-loop and if-statement to check [truthiness](https://paulapivat.com/post/dsfs_2/#truthiness) of `user_interest == target_interest`. 

```python
def num_user_with_interest_in(target_interest):
    interest_count = 0
    for user_id, user_interest in interests:
        if user_interest == target_interest:
            interest_count += 1
    return interest_count
```

A concern is having to examine a whole list of interests for every search. The author proposes building an index from interests to users. Here, a [defaultdict](https://paulapivat.com/post/dsfs_2/#defaultdict) is imported, then populated with user_id

```python
from collections import defaultdict

# user_ids matched to specific interest
user_ids_by_interest = defaultdict(list)

for user_id, interest in interests:
    user_ids_by_interest[interest].append(user_id)

# three users interested in Python
assert user_ids_by_interest["Python"] == [2,3,5]

# list of interests by user_id
interests_by_user_id = defaultdict(list)

for user_id, interest in interests:
    interests_by_user_id[user_id].append(interest)

# check all of Hero's interests
assert interests_by_user_id[0] == ['Hadoop', 'Big Data', 'HBase', 'Java', 'Spark', 'Storm', 'Cassandra']
```
We can find who has the most interests in common with a given user. Looks like Klein (#9) has the most common interests with Hero (#0). Here we return a Counter with for-loops and an if-statement. 

```python
def most_common_interests_with(user):
    return Counter(
        interested_user_id
        for interest in interests_by_user_id[user["id"]]
        for interested_user_id in user_ids_by_interest[interest]
        if interested_user_id != user["id"]
        )
        
# let's check to see who has the most common interest with Hero
most_common_interests_with(users[0]) # Counter({9: 3, 8: 1, 1: 2, 5: 1})
```

Finally, we can also find which topics are most popular among the network. Previously, we calculated the number of users interested in a particular topic, but now we want to compare the whole list. 

```python
words_and_counts = Counter(word
                           for user, interest in interests
                           for word in interest.lower().split())
```

#### Salaries and Experience Data

We're also given anonymous salary and tenure (number of years work experience) data, let's see what we can do with that information. First we'll find the average salary. Again, we'll start by creating a list (defaultdict), then loop through `salaries_and_tenures`.

```python
salaries_and_tenures = [(83000, 8.7), (88000, 8.1),
                        (48000, 0.7), (76000, 6),
                        (69000, 6.5), (76000, 7.5),
                        (60000, 2.5), (83000, 10),
                        (48000, 1.9), (63000, 4.2)]
                        
salary_by_tenure = defaultdict(list)

for salary, tenure in salaries_and_tenures:
    salary_by_tenure[tenure].append(salary)

# find average salary by tenure
average_salary_by_tenure = {
    tenure: sum(salaries) / len(salaries)
    for tenure, salaries in salary_by_tenure.items()
    }
```

The problem is that this is not terribly informative as each tenure value has a different salary. Not even the `average_salary_by_tenure` is informative, so our next move is to group similar tenure values together. 

First, we'll create the groupings/categories using a [control-flow](https://paulapivat.com/post/dsfs_2/#controlflow), then we'll create a `list`(`defaultdict`), and loop through `salaries_and_tenures` to populate the newly created `salary_by_tenure_bucket`. Finally calculate the average.

```python
def tenure_bucket(tenure):
    if tenure < 2:
        return "less than two"
    elif tenure < 5:
        return "between two and five"
    else:
        return "more than five"
        
salary_by_tenure_bucket = defaultdict(list)

for salary, tenure in salaries_and_tenures:
    bucket = tenure_bucket(tenure)
    salary_by_tenure_bucket[bucket].append(salary)
    
# finally calculate average
average_salary_by_bucket = {
    tenure_bucket: sum(salaries) / len(salaries)
    for tenure_bucket, salaries in salary_by_tenure_bucket.items()
    }
```

One thing to note is that the "given" data, in this hypothetical toy example is either in a list of dictionaries or tuples, which may be atypical if we're used to working with tabular data in dataFrame (pandas) or native data.frame in R.

Again, we are reminded that the higher purpose of this book - Data Science from Scratch (by Joel Grus; 2nd Ed) is to eschew libraries in favor of plain python to build everything from the ground up.

Should your goal be to learn how various algorithms work by building them up from scratch, and in the process learn how data problems can be solved with python and minimal libraries, this is your book.

Joel Grus does make clear that you would use libraries and frameworks (pandas, scikit-learn, matplotlib etc), rather than coded-from-scratch algorithms when working in production environments and will point out resource for further reading at the end of the chapters.

In the next post, we'll get into visualizing data.