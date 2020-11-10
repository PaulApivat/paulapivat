---
authors:
- admin
categories: []
date: "2020-11-10T00:00:00Z"
draft: false
featured: false
image:
  caption: ""
  focal_point: ""
lastMod: "2020-11-10T00:00:00Z"
projects: []
subtitle: Vectors & Matrices
summary: Building tools for working with vectors and matrices from scratch
tags: []
title: Data Science from Scratch (ch4) - Linear Algebra
---

### Table of contents
- [Vectors](#vectors)
- [Matrices](#matrices)

## Overview

We'll see the **from scratch** aspect of the book play out as we implement several building block functions to help us work towards defining the **Euclidean Distance** in code:

![png](./distance_vectors.png)

While we don't see its application immediately, we can expect to see the **Euclidean Distance** used for K-nearest neighbors (classication) or K-means (clustering) to find the "k closest points" ([source](https://sebastianraschka.com/faq/docs/euclidean-distance.html#:~:text=Machine%20Learning%20FAQ&text=For%20example%2C%20picture%20it%20as,of%20a%20particular%20sample%20point.)). (*note* : there are other types of distance formulas used as well.)

En route towards implementing the **Euclidean Distance**, we also implement the **sum of squares** which is a crucial piece for how **regression** works. 

Thus, the **from scratch** aspect of this book works on two levels. *Within* this chapter, we're building piece by piece up to an important **distance** and **sum of squares** formula. But we're also building tools we'll use in subsequent chapters. 

## Vectors

We start off with implementing functions to **add** and **subtract** two vectors. We also create a function for *component wise sum* of a list of vectors, where a new vector is created whose first element is the sum of all the first elements in the list and so on. 

We then create a function to **multiply** a vector by  scalar, which we use to compute the *component wise mean* of a list of vectors. 

We also create the **dot product** of two vectors or the *sum of their component wise product*, and this is is the generalize version of the **sum of squares**. At this point, we have enough to implement the **Euclidean distance**. Let's take a look at the code:

#### Example Vectors

Vectors are simply a list of numbers:

```python
height_weight_age = [70,170,40]

grades = [95,80,75,62]
```

### Add

You'll *note* that we do **type annotation** on our code throughout. This is a convention advocated by the author (and as a newcomer to Python, I like the idea of being explicit about data type for a function's input and output). 

```python
from typing import List

Vector = List[float]

def add(v: Vector, w: Vector) -> Vector:
    """Adds corresponding elements"""
    assert len(v) == len(w), "vectors must be the same length"
    return [v_i + w_i for v_i, w_i in zip(v,w)]
    
assert add([1,2,3], [4,5,6]) == [5,7,9]
```
Here's another view of what's going on with the `add` function:

![add](./add.png)

### Subtract

```python
def subtract(v: Vector, w: Vector) -> Vector:
    """Subtracts corresponding elements"""
    assert len(v) == len(w), "vectors must be the same length"
    return [v_i - w_i for v_i, w_i in zip(v,w)]
    
assert subtract([5,7,9], [4,5,6]) == [1,2,3]
```
This is pretty much the same as the previous:

![subtract](./subtract.png)

### Componentwise Sum

```python
def vector_sum(vectors: List[Vector]) -> Vector:
    """Sum all corresponding elements (componentwise sum)"""
    # Check that vectors is not empty
    assert vectors, "no vectors provided!"
    # Check the vectorss are all the same size
    num_elements = len(vectors[0])
    assert all(len(v) == num_elements for v in vectors), "different sizes!"
    # the i-th element of the result is the sum of every vector[i]
    return [sum(vector[i] for vector in vectors)
            for i in range(num_elements)]
            
assert vector_sum([[1,2], [3,4], [5,6], [7,8]]) == [16,20]
```
Here, a `list` of vectors becomes *one* vector. If you go back to the `add` function, it takes **two** vectors, so if we tried to give it four vectors, we'd get a `TypeError`. So we wrap four vectors in a `list` and provide *that* as the argument for `vector_sum`:

![vector_sum](./vector_sum.png)

### Multiply Vector with a Number

```python
def scalar_multiply(c: float, v: Vector) -> Vector:
    """Multiplies every element by c"""
    return [c * v_i for v_i in v]
    
assert scalar_multiply(2, [2,4,6]) == [4,8,12]
```
One number is multiplied with *all* numbers in the vector, with the vector retaining its length:

![multiply](./multiply.png)


### Componentwise Mean

This is similar to componentwise sum (see above); a list of vectors becomes one vector.

```python
def vector_mean(vectors: List[Vector]) -> Vector: 
    """Computes the element-wise average"""
    n = len(vectors)
    return scalar_multiply(1/n, vector_sum(vectors))
    
assert vector_mean([ [1,2], [3,4], [5,6] ]) == [3,4]
```


### Dot Product

```python
def dot(v: Vector, w: Vector) -> float:
    """Computes v_1 * w_1 + ... + v_n * w_n"""
    assert len(v) == len(w), "vectors must be the same length"
    return sum(v_i * w_i for v_i, w_i in zip(v,w))
    
assert dot([1,2,3], [4,5,6]) == 32
```
Here we multiply the elements, then sum their results. Two vectors becomes a single number (`float`):

![dot_product](./dot_product.png)

### Sum of Squares

```python
def sum_of_squares(v: Vector) -> float:
    """Returns v_1 * v_1 + ... + v_n * v_n"""
    return dot(v,v)
    
assert sum_of_squares([1,2,3]) == 14
```
In fact, `sum_of_squares` is a special case of **dot product**:

![sum_of_squares](./sum_of_squares.png)

### Magnitude

```python
def magnitude(v: Vector) -> float:
    """Returns  the magnitude (or length) of v"""
    return math.sqrt(sum_of_squares(v)) # math.sqrt is the square root function
    
assert magnitude([3,4]) == 5
```
With `magnitude` we square root the `sum_of_squares`. This is none other than the [pythagorean theorem](https://en.wikipedia.org/wiki/Pythagorean_theorem). 

![magnitude](./magnitude.png)


### Squared Distance 

```python
def squared_distance(v: Vector, w: Vector) -> float:
    """Computes (v_1 - w_1) ** 2 + ... + (v_n - w_n) ** 2"""
    return sum_of_squares(subtract(v,w))
```
This is the distance *between* two vectors, *squared*.

![squared_distance](./squared_distance.png)

### (Euclidean) Distance

```python
import math

def distance(v: Vector, w: Vector) -> float:
    """Also computes the distance between v and w"""
    return math.sqrt(squared_distance(v,w))
```

Finally, we square root the `squared_distance` to get the (euclidean) distance:

![distance_vectors](./distance_vectors.png)

### Summary

We literally built from scratch, albeit with some help from Python's `math` module, the blocks needed for essential functions that we'll expect to use later, namely: the `sum_of_squares` and `distance`.

It's pretty cool to see these foundational concepts set us up to understand more complex machine learning algorithms like **regression**, **k-nearest neighbors (classification)**, **k-means (clustering)** and even touch on the **pythagorean theorem**. 

We'll examine matrices next. 


