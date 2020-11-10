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
height_weight_age = [70,
                    170,
                    40]

grades = [95,
          80,
          75,
          62]
```

#### Add

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

#### Subtract

```python
def subtract(v: Vector, w: Vector) -> Vector:
    """Subtracts corresponding elements"""
    assert len(v) == len(w), "vectors must be the same length"
    return [v_i - w_i for v_i, w_i in zip(v,w)]
    
assert subtract([5,7,9], [4,5,6]) == [1,2,3]
```

#### Componentwise Sum

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

#### Multiply Vector with a Number

```python
def scalar_multiply(c: float, v: Vector) -> Vector:
    """Multiplies every element by c"""
    return [c * v_i for v_i in v]
    
assert scalar_multiply(2, [2,4,6]) == [4,8,12]
```

#### Componentwise Mean

```python
def vector_mean(vectors: List[Vector]) -> Vector: 
    """Computes the element-wise average"""
    n = len(vectors)
    return scalar_multiply(1/n, vector_sum(vectors))
    
assert vector_mean([ [1,2], [3,4], [5,6] ]) == [3,4]
```

#### Dot Product

```python
def dot(v: Vector, w: Vector) -> float:
    """Computes v_1 * w_1 + ... + v_n * w_n"""
    assert len(v) == len(w), "vectors must be the same length"
    return sum(v_i * w_i for v_i, w_i in zip(v,w))
    
assert dot([1,2,3], [4,5,6]) == 32
```

### Sum of Squares

```python
def sum_of_squares(v: Vector) -> float:
    """Returns v_1 * v_1 + ... + v_n * v_n"""
    return dot(v,v)
    
assert sum_of_squares([1,2,3]) == 14
```
### Magnitude

```python
def magnitude(v: Vector) -> float:
    """Returns  the magnitude (or length) of v"""
    return math.sqrt(sum_of_squares(v)) # math.sqrt is the square root function
    
assert magnitude([3,4]) == 5
```

### Squared Distance 

```python
def squared_distance(v: Vector, w: Vector) -> float:
    """Computes (v_1 - w_1) ** 2 + ... + (v_n - w_n) ** 2"""
    return sum_of_squares(subtract(v,w))
```

### Distance

```python
def distance(v: Vector, w: Vector) -> float:
    """Also computes the distance between v and w"""
    return magnitude(subtract(v,w))
```
