---
authors:
- admin
categories: []
date: "2020-10-23T00:00:00Z"
draft: false
featured: false
image:
  caption: ""
  focal_point: ""
lastMod: "2020-10-29T00:00:00Z"
projects: []
subtitle: Python crash course
summary: Survey of python features relevant for Data Science
tags: []
title: Data Science from Scratch (ch2)
---

**Table of Content:**
- [Set Up](#setup)
- [Functions](#functions)
- [Strings](#strings)
- [Exceptions](#exceptions)
- [Lists](#lists)
- [Tuples](#tuples)
- [Dictionaries](#dictionaries)
- [defaultdict](#defaultdict)

## Chapter 2: A Crash Course in Python


This is the first of many chapters i'll be covering from Joel Grus' Data Science from Scratch book (2nd edition). This chapter provides a quick survey of python features needed for "doing" data science from scratch, including essential setup of virtual environments and other tooling.

While the chapter is not meant to be comprehensive, I may supplement certain sections with external content for greater detail in certain parts. 

My goal is twofold. First, to go through this book and, as a byproduct, learn python. Second, to look out for and highlight the areas where the *pythonic* way of doing things is necessary to accomplish something in the data science process. 

At several sections throughout this chapter, the author emphasises how much a particular feature will be used later in the book (e.g., functions, dictionaries, list, list comprehensions (and for-loops), assert, iterables and generators, randomness, type annotations). Things *not* used as much (e.g., sets, automated test, subclasses that inherit functionality from a parent class, zip and argument unpacking, args, kwargs).

Additional code can be found in this [repo](https://github.com/PaulApivat/dsfs)

## Setup
### Installation, Virtual Environment and Modules

These section takes the reader through installing a virtual environment using Anaconda Python distribution. The author points out a best practice, "you should always work in a virtual environment and never use 'base' Python installation". Moreover, the author favors IPython over jupyter notebooks (he's a noted [critic of the notebook](https://www.youtube.com/watch?v=7jiPeIFXb6U))

The first time I installed Python, it took me awhile to get things right and eventually I relied on jupyter notebooks through Anaconda. As we go through this book, I'll be using virtual environments and IPython as much as I can (although I may sprinkle in a notebook here and there). My IDE for interacting with the conda virtual environment and IPython will be VSCode. 

Fortunately, I had a relatively painless process setting up a virtual environment and IPython, although I had to take a slight detour to setup the `code` command for VSCode.

Here's a summary of the commands I used for setup:

![png](./python_virtual_env.png)

## Functions

Three things are emphasized here: passing functions as arguments for other functions, lambda functions and default parameter values. 

The illustration of functions being passed as arguments is demonstrated below. A function `double` is created. A function `apply_to_one` is created. The `double` function is pointed at `my_double`. We pass `my_double` into the `apply_to_one` function and set that to `x`. 

Whatever function is passed to `apply_to_one`, *its* argument is 1. So passing `my_double` means we are doubling 1, so x is 2.

But the important thing is that a function got passed to another function (aka higher order functions).

```
def double(x):
    """
    this function doubles and returns the argument
    """
    return x * 2
    
def apply_to_one(f):
    """Calls the function f with 1 as its argument"""
    return f(1)
    
my_double = double

# x is 2 here
x = apply_to_one(my_double)

# extending this example
def apply_five_to(e):
    """returns the function e with 5 as its argument"""
    return e(5)

# doubling 5 is 10
w = apply_five_to(my_double)
```

Since functions are going to be used extensively, here's another more complicated example. I found this from [Trey Hunner's site](https://treyhunner.com/2020/01/passing-functions-as-arguments/). Two functions are defined - `square` and `cube`. Both functions are saved to a list called `operations`. Another list, `numbers` is created. 

Finally, a for-loop is used to iterate through `numbers`, and the `enumerate` property allows access to both index and item in numbers. That's used to find whether the `action` is a `square` or `cube` (operations[0] is `square`, operations[1] is `cube`), which is then given as its argument, the items inside the `numbers` list. 

```
# create two functions
def square(n): return n**2
def cube(n): return n**3

# store those functions inside a list, operations, to reference later
operations = [square, cube]

# create a list of numbers
numbers = [2,1,3,4,7,11,18,29]

# loop through the numbers list
# using enumerate the identify index and items
# [i % 2] results in either 0 or 1, that's pointed at action
# using the dunder, name, retrieves the name of the function - either square or cube - from the operations list
# print __name__ along with the item from the numbers list
# action is either a square or cube

for i, n in enumerate(numbers):
    action = operations[i % 2]
    print(f"{action.__name__}({n}):", action(n))

# print
square(2): 4
cube(1): 1
square(3): 9
cube(4): 64
square(7): 49
cube(11): 1331
square(18): 324
cube(29): 24389

# more explicit, yet verbose way to write the for-loop
for index, num in enumerate(numbers):
    action = operations[index % 2]
    print(f"{action.__name__}({num}):", action(num))

```

This section also introduces `lambda` functions (aka anonymous functions) to demonstrate how functions, being first-class in Python, can, like any variable, be passed into the argument of another function. However, with `lambda` instead of defining functions with `def`, it is defined inside another function. Here's an illustration:

```
# we'll reuse apply_five_to, which takes in a function and provides '5' as the argument
def apply_five_to(e):
    """returns the function e with 5 as its argument"""
    return e(5)

# this lambda function adds '4' to any argument
# when passing this lambda function to apply_five_to
# you get y = 5 + 4
y = apply_five_to(lambda x: x + 4)

# we can also change what the lambda function does without defining a separate function
# here the lambda function multiplies the argument by 4
# y = 20
y = apply_five_to(lambda x: x * 4)

```
Lambda functions are convenient in that you can pass it into another function *immediately* without having to define it separately, but the consensus seems to be that you should just use `def`.

Here's an external example of `lambda` functions from [Trey Hunner](https://treyhunner.com/2020/01/passing-functions-as-arguments/). In this example, a `lambda` function is used within a `filter` function that takes in two arguments.

```
# calling help(filter) displays an explanation

class filter(object)
 |  filter(function or None, iterable) --> filter object

# create a list of numbers
numbers = [2,1,3,4,7,11,18,29]

# the lambda function will return n if it is an even number
# we filter the numbers list using the lambda function
# wrapped in a list, this returns [2,4,18]
list(filter(lambda n: n % 2 == 0, numbers))

```

There are whole books, or at least whole chapters, that can be written about Python functions, but we'll limit our discussion for now to the idea that **functions can be passed as arguments to other functions**. I'll report back on this section as we progress through the book.

## Strings

Strings may not be terribly exciting for data science or machine learning, unless you're getting into natural language processing, so we'll keep it brief here. The key take aways are that *backslashes* encode special characters and that **f-strings** is the most updated way to do string interpolation. Here are some examples:

```python
# point strings to variables (we'll use my name)
first_name = "Paul"
last_name = "Apivat"

# f-string (recommended), 'Paul Apivat'
f_string = f"{first_name} {last_name}"

# string addition, 'Paul Apivat'
string_addition = first_name + " " + last_name

# string format, 'Paul Apivat'
string_format = "{0} {1}".format(first_name, last_name)

# percent format (NOT recommended), 'Paul Apivat'
pct_format = "%s %s" %('Paul','Apivat')
```

## Exceptions

The author covers exceptions to make the point that they're not all that bad in Python and it's worth handling exceptions yourself to make code more readable. Here's my own example that's slightly different from the book:

```python
integer_list = [1,2,3]

heterogeneous_list = ["string", 0.1, True]

# you can sum a list of integers, here 1 + 2 + 3 = 6
sum(integer_list)

# but you cannot sum a list of heterogeneous data types
# doing so raises a TypeError
sum(heterogeneous_list)

# the error crashes your program and is not fun to look at
---------------------------------------------------------------------------
TypeError                                 Traceback (most recent call last)
<ipython-input-12-3287dd0c6c22> in <module>
----> 1 sum(heterogeneous_list)

TypeError: unsupported operand type(s) for +: 'int' and 'str'

# so the idea is to handle the exceptions with your own messages
try:
    sum(heterogeneous_list)
except TypeError:
    print("cannot add objects of different data types")
```

At this point, the primary benefits to handling exceptions yourself is for code readability, so we'll come back to this section if we see more useful examples.  

## Lists 

Lists are fundamental to Python so I'm going to spend some time exploring their features. For data science, `NumPy arrays` are used frequently, so I thought it'd be good to implement all `list` operations covered in this section in `Numpy arrays` to *tease apart their similarities and differences*. 

Below are the similarities. 

This implies that whatever can be done in python `lists` can also be done in numpy `arrays`, including: getting the *nth* element in the list/array with square brackets, slicing the list/array, iterating through the list/array with *start, stop, step*, using the `in` operator to find list/array membership, checking length and unpacking list/arrays. 

```python
# setup
import numpy as np

# create comparables
python_list = [1,2,3,4,5,6,7,8,9]
numpy_array = np.array([1,2,3,4,5,6,7,8,9])

# bracket operations

# get nth element with square bracket
python_list[0] # 1
numpy_array[0] # 1
python_list[8] # 9
numpy_array[8] # 9
python_list[-1] # 9
numpy_array[-1] # 9

# square bracket to slice 
python_list[:3] # [1, 2, 3]
numpy_array[:3] # array([1, 2, 3])

python_list[1:5] # [2, 3, 4, 5]
numpy_array[1:5] # array([2, 3, 4, 5])

# start, stop, step
python_list[1:8:2] # [2, 4, 6, 8]
numpy_array[1:8:2] # array([2, 4, 6, 8])

# use in operator to check membership
1 in python_list # true
1 in numpy_array # true

0 in python_list # false
0 in numpy_array # false

# finding length
len(python_list) # 9
len(numpy_array) # 9

# unpacking
x,y = [1,2] # now x is 1, y is 2
w,z = np.array([1,2]) # now w is 1, z is 2


```

Now, here are the differences. 

These tasks can be done in python `lists`, but require a different approach for NumPy `array` including: modification (extend in list, append for array). Finally, lists can store mixed data types, while NumPy array will convert to string. 

```python

# python lists can store mixed data types
heterogeneous_list = ['string', 0.1, True]
type(heterogeneous_list[0]) # str
type(heterogeneous_list[1]) # float
type(heterogeneous_list[2]) # bool

# numpy arrays cannot store mixed data types
# numpy arrays turn all data types into strings
homogeneous_numpy_array = np.array(['string', 0.1, True]) # saved with mixed data types
type(homogeneous_numpy_array[0]) # numpy.str_
type(homogeneous_numpy_array[1]) # numpy.str_
type(homogeneous_numpy_array[2]) # numpy.str_


# modifying list vs numpy array

# lists can use extend to modify list in place
python_list.extend([10,12,13])  # [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 12, 13]
numpy_array.extend([10,12,13]) # AttributeError: 'numpy.ndarray'

# numpy array must use append, instead of extend
numpy_array = np.append(numpy_array,[10,12,13])

# python lists can be added with other lists
new_python_list = python_list + [14,15] # [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 12, 13, 14, 15]
numpy_array + [14,15] # ValueError

# numpy array cannot be added (use append instead)
# array([ 1,  2,  3,  4,  5,  6,  7,  8,  9, 10, 12, 13, 14, 15])
new_numpy_array = np.append(numpy_array, [14,15]) 

# python lists have the append attribute
python_list.append(0) # [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 12, 13, 0]

# the append attribute for numpy array is used differently
numpy_array = np.append(numpy_array, [0])

```

Python `lists` and NumPy `arrays` have much in common, but there are meaningful differences as well.

#### Python Lists vs NumPy Arrays: What's the difference

Now that we know that there *are* meaningful differences, what can we attribute these differences to? This [explainer from UCF](https://webcourses.ucf.edu/courses/1249560/pages/python-lists-vs-numpy-arrays-what-is-the-difference) highlights **performance** differences including:

- Size
- Performance
- Functionality

I'm tempted to go down this 🐇 🕳️ of further `lists` vs `array` comparisons, but we'll hold off for now.


## Tuples

Similar to `lists`, but `tuples` are immutable. 

```python

my_list = [1,2]   # check type(my_list)
my_tuple = (1,2)  # check type(my_tuple)
other_tuple = 3,4 # tuples don't require parentheses

my_list[1] = 3    # lists ARE mutable, my_list is now [1,3]

# exception handling when trying to change tuple
try:
    my_tuple[1] = 3
except TypeError:
    print("tuples are immutable")

```

`Tuples` are good at returning multiple values from functions:

```python

# use tuple to return multiple values
def sum_and_product(x,y):
    """you can return multiple values from functions using tuples"""
    return (x + y), (x * y)
    
sp = sum_and_product(4,5)  # sp is (9,20), a tuple

```
However, `lists` can also be used to return multiple values:

```python

def sum_and_product_list(x,y):
    return [(x + y), (x * y)]

spl = sum_and_product_list(5,6)  # [11, 30]
type(spl) # list
```
Finally, both `tuples` and `lists` can be used for multiple assignments, here's a pythonic way to swap variables:

```python
x, y = 1,2
x,y = y,x
```

Tuples, for the most part, seem to be redundant with `lists`, but we'll see if there are special use-cases for immutability down the line. 

## Dictionaries

Dictionaries are good for storing structured data. They have a key/value pair so you can look up values of certain keys. The author provides some ways to initialize a dictionary, with comments about what is *more or less pythonic* (I'll take the author's word for it, but open to other perspectives).

Some of the things you can do with `dictionaries` are query keys, values, assign new key/value pairs, check for existence of keys or retrieve certain values.

```python

empty_dict = {}                   # most pythonic
empty_dict2 = dict()              # less pythonic
grades = {"Joel": 80, "Grus": 99} # dictionary literal

type(grades)  # type check, dict

# use bracket to look up values
grades["Grus"]  # 99
grades["Joel"]  # 80

# KeyError for looking up non-existent keys
try:
   kate_grades = grades["Kate"]
except KeyError:
   print("That key doesn't exist")
   
# use in operator to check existence of key
joe_has_grade = "Joel" in grades  
joe_has_grade # true

kate_does_not = "Kate" in grades
kate_does_not # false

# use 'get' method to get values in dictionaries
grades.get("Joel") # 80
grades.get("Grus") # 99
grades.get("Kate") # default: None

# assign new key/value pair using brackets
grades["Tim"] = 93

grades # {'Joel': 80, 'Grus': 99, 'Tim': 93}

```

Dictionaries are good for representing structured data that can be queries. The key take-away here is that in order to iterate through `dictionaries` to get either `keys`, `values` or both, we'll need to use specific methods likes `keys()`, `values()` or `items()`.

```python

tweet = {
    "user": "paulapivat",
    "text": "Reading Data Science from Scratch",
    "retweet_count": 100,
    "hashtags": ["#66daysofdata", "datascience", "machinelearning", "python", "R"]
    }
    
# query specific values
tweet["retweet_count"] # 100

# query values within a list
tweet["hashtags"] # ['#66daysofdata', 'datascience', 'machinelearning', 'python', 'R']
tweet["hashtags"][2] # 'machinelearning'

# retrieve ALL keys
tweet_keys = tweet.keys()
tweet_keys              # dict_keys(['user', 'text', 'retweet_count', 'hashtags'])
type(tweet_keys)        # different data type: dict != dict_keys

# retrieve ALL values
tweet_values = tweet.values() 
tweet_values  # dict_values(['paulapivat', 'Reading Data Science from Scratch', 100, ['#66daysofdata', 'datascience', 'machinelearning', 'python', 'R']])

type(tweet_values)      # different data type: dict != dict_values

# create iterable for Key-Value pairs (in tuple)
tweet_items = tweet.items()

# iterate through tweet_items()
for key,value in tweet_items:
    print("These are the keys:", key)
    print("These are the values:", value)
    
# cannot iterate through original tweet dictionary
# ValueError: too many values to unpack (expected 2)
for key, value in tweet:
    print(key)
    
# cannot use 'enumerate' because that only provides index and key (no value)
for key, value in enumerate(tweet):
    print(key)   # print 0 1 2 3 - index values
    print(value) # user text retweet_count hashtags (incorrectly print keys)
```

Just like in `lists` and `tuples`, you can use the `in` operator to find membership. The one caveat is you cannot look up *values* that are in `lists`, unless you use bracket notation to help.

```python

# search keys
"user" in tweet # true
"bball" in tweet # false

"paulapivat" in tweet_values # true
'python' in tweet_values # false (python is nested in 'hashtags')
"hashtags" in tweet  # true

# finding values inside a list requires brackets to help
'python' in tweet['hashtags']  # true

```

**What is or is not hashable?**

`Dictionary` keys must be hashable.

`Strings` are hashable. So we can use `strings` as dictionary keys, but we **cannot** use `lists` because they are not hashable.

```python

paul = "paul"
type(paul)        # check type, str

hash(paul)        # -3897810863245179227 ; strings are hashable
paul.__hash__()   # -3897810863245179227 ; another way to find the hash

jake = ['jake']   # this is a list
type(jake)        # check type, list

# lists are not hashable - cannot be used as dictionary keys
try:
   hash(jake)
except TypeError:
   print('lists are not hashable')

```


## defaultdict

`defaultdict` is a **subclass** of dictionaries (`dict`, see previous post), so it *inherits* most of its behavior from `dict` with additional features. To understand how those features make it different, and more convenient in some cases, we'll need to run into some errors. 

If we try to count words in a document, the general approach is to create a dictionary where the dictionary `keys` are words and the dictionary `values` are counts of those words. 

Let's try do do this with a regular dictionary. 

First, to setup, we'll take a list of words and `split()` into individual words. I took this paragraph from [another project](https://rpubs.com/paulapivat/vintage_nba_seasons) i'm working on and artificially added some extra words to ensure that certain words appeared more than once (it'll be apparent why soon).

```python

# paragraph
lines = ["This table highlights 538's new NBA statistic, RAPTOR, in addition to the more established Wins Above Replacement (WAR). An extra column, Playoff (P/O) War, is provided to highlight stars performers in the post-season, when the stakes are higher. The table is limited to the top-100 players who have played at least 1,000 minutes minutes the table Wins NBA NBA RAPTOR more players"]

# split paragraphy into individual words
lines = " ".join(lines).split()

type(lines) # list
```

Now that we have our `lines` list, we'll create an empty `dict` called `word_counts` and have each word be the `key` and each `value` be the count of that word.

```python
# empty list
word_counts = {}

# loop through lines to count each word
for word in lines:
    word_counts[word] += 1
    
# KeyError: 'This'
```
We received a `KeyError` for the very first word in `lines` (i.e. 'This') because the **list tried to count a key that didn't exist**. We've learned to handle exceptions so we can use `try` and `except`.

Here, we're looping through `lines` and when we try to count a key that doesn't exist, like we did previously, we're *now* anticipating a `KeyError` and will set the initial count to 1, then it can continue to loop-through and count the word, which now exists, so it can be incremented up. 

```python
# empty list
word_counts = {}

# exception handling
for word in lines:
    try:
        word_counts[word] += 1
    except KeyError:
        word_counts[word] = 1

# call word_counts
# abbreviated for space
word_counts

{'This': 1,
 'table': 3,
 'highlights': 1,
 "538's": 1,
 'new': 1,
 'NBA': 3,
 'statistic,': 1,
 'RAPTOR,': 1,
 'in': 2,
 'addition': 1,
 'to': 3,
 'the': 5,
 'more': 2,
 ...
 'top-100': 1,
 'players': 2,
 'who': 1,
 'have': 1,
 'played': 1,
 'at': 1,
 'least': 1,
 '1,000': 1,
 'minutes': 2,
 'RAPTOR': 1}
```
Now, there are other ways to achieve the above:

```python
# use conditional flow
word_counts = {}

for word in lines:
    if word in word_counts:
        word_counts[word] += 1
    else:
        word_counts[word] = 1
        
# use get
for word in lines:
    previous_count = word_counts.get(word, 0)
    word_counts[word] = previous_count + 1
```

Here's where the author makes the case for `defaultdict`, arguing that the two aforementioned approaches are unweildy. We'll come back full circle to try our first approach, using `defaultdict` instead of the traditional `dict`.

`defaultdict` is a subclass of `dict` and must be imported from `collections`:

```python
from collections import defaultdict

word_counts = defaultdict(int)

for word in lines:
    word_counts[word] += 1
    
# we no longer get a KeyError
# abbreviated for space
defaultdict(int,
            {'This': 1,
             'table': 3,
             'highlights': 1,
             "538's": 1,
             'new': 1,
             'NBA': 3,
             'statistic,': 1,
             'RAPTOR,': 1,
             'in': 2,
             'addition': 1,
             'to': 3,
             'the': 5,
             'more': 2,
             ...
             'top-100': 1,
             'players': 2,
             'who': 1,
             'have': 1,
             'played': 1,
             'at': 1,
             'least': 1,
             '1,000': 1,
             'minutes': 2,
             'RAPTOR': 1})

```
Unlike a regular dictionary, when `defaultdict` tries to look up a key it doesn't contain, it'll automatically add a value for it using the argument we provided when we first created the `defaultdict`. If you see above, we entered an `int` as the argument, which allows it to automatically *add a value*. 


### Counters

### Sets

### Control Flow

### Truthiness

### Sorting

### List Comprehensions

### Automated Testing and assert

### Object-Oriented Programming

### Iterables and Generators

### Randomness

### Regular Expressions

### zip and Argument Unpacking

### args and kwargs

### Type Annotations

### How to Write Type Annotations


###






