---
authors:
- admin
categories: []
date: "2020-12-15T00:00:00Z"
draft: false
featured: false
image:
  caption: ""
  focal_point: ""
lastMod: "2020-12-15T00:00:00Z"
projects: []
subtitle: Connecting probability and statistics to hypothesis testing and inference
summary: An overview of statistical Hypothesis Testing, Estimation and Bayesian Inference
tags: ["Python", "Data Science", "Probability", "Statistics"]
title: Data Science from Scratch (ch7) - Hypothesis and Inference
---

### Table of contents

- [Central Limit Theorem](#central_limit_theorem)
- [Hypothesis Testing](#hypothesis_testing)

## Overview

We'll use a classic coin-flipping example in this post because it is simple to illustrate with both **concept** and **code**. My goal is to tie several concepts together including (traditional) Hypothesis and Inference, Estimation Theory, and Bayesian Inference. All using the same coin-flipping example. 

## Central_Limit_Theorem

Terms like "null" and "alternative" hypothesis are used quite frequently, so let's set some context. The "null" is the **default** position. The "alternative", alt for short, is something we're *comparing to* the default (null). 

The classic coin-flipping exercise is to test the *fairness* off a coin. If a coin is fair, it'll land on heads 50% of the time (and tails 50% of the time). Let's translate into hypothesis testing language:

**Null Hypothesis**: Probability of landing on Heads = 0.5.

**Alt Hypothesis**: Probability of landing on Heads != 0.5.

Each coin flip is a **Bernoulli trial**, which is an experiment with two outcomes - outcome 1, "success", (probability *p*) and outcome 0, "fail" (probability *p - 1*). The reason it's a Bernoulli trial is because there are only two outcome with a coin flip (heads or tails). Read more about [Bernoulli here](https://en.wikipedia.org/wiki/Bernoulli_trial). 

Here's the code for a single Bernoulli Trial:

```python
def bernoulli_trial(p: float) -> int:
    """Returns 1 with probability p and 0 with probability 1-p"""
    return 1 if random.random() < p else 0
```

When you **sum the independent Bernoulli trials**, you get a **Binomial(n,p)** random variable, a variable whose *possible* values have a probability distribution. The **central limit theorem** says as **n** or the *number* of independent Bernoulli trials get large, the Binomial distribution approaches a normal distribution.

Here's the code for when you sum all the Bernoulli Trials to get a Binomial random variable:

```python
def binomial(n: int, p: float) -> int:
    """Returns the sum of n bernoulli(p) trials"""
    return sum(bernoulli_trial(p) for _ in range(n))
```

**Note**: A single 'success' in a Bernoulli trial is 'x'. Summing up all those x's into X, is a Binomial random variable. Success doesn't imply desirability, nor does "failure" imply undesirability. They're just terms to count the cases we're looking for (i.e., number of heads in multiple coin flips to assess a coin's fairness).

Given that our **null** is (p = 0.5) and **alt** is (p != 0.5), we can run some independent bernoulli trials, then sum them up to get a binomial random variable. 

![independent_coin_flips](./independent_coin_flips.png)

Each `bernoulli_trial` is an experiment with either 0 or 1 as outcomes. The `binomial` function sums up **n** bernoulli(0.5) trails. We ran both twice and got different results. Each bernoulli experiment can be a success(1) or fial(0); summing up into a binomial random variable means we're taking the probability p(0.5) *that a coin flips head* and we ran the experiment 1000 times to get a random binomial variable. 

The first 1000 flips we got 510. The second 1000 flips we got 495. We can repeat this process many times to get a *distribution*. We can plot this distribution to reinforce our understanding. To this we'll use `binomial_histogram` function. This function picks points from a Binomial(n,p) random variable and plots their histogram.

```python
from collections import Counter
import matplotlib.pyplot as plt

def normal_cdf(x: float, mu: float = 0, sigma: float = 1) -> float:
    return (1 + math.erf((x - mu) / math.sqrt(2) / sigma)) / 2
    

def binomial_histogram(p: float, n: int, num_points: int) -> None:
    """Picks points from a Binomial(n, p) and plots their histogram"""
    data = [binomial(n, p) for _ in range(num_points)]
    # use a bar chart to show the actual binomial samples
    histogram = Counter(data)
    plt.bar([x - 0.4 for x in histogram.keys()],
            [v / num_points for v in histogram.values()],
            0.8,
            color='0.75')
    mu = p * n
    sigma = math.sqrt(n * p * (1 - p))
    # use a line chart to show the normal approximation
    xs = range(min(data), max(data) + 1)
    ys = [normal_cdf(i + 0.5, mu, sigma) -
          normal_cdf(i - 0.5, mu, sigma) for i in xs]
    plt.plot(xs, ys)
    plt.title("Binomial Distribution vs. Normal Approximation")
    plt.show()

# call function   
binomial_histogram(0.5, 1000, 10000)
```

This plot is then rendered:

![binomial_coin_fairness](./binomial_coin_fairness.png)

What we did was sum up independent `bernoulli_trial`(s) of 1,000 coin flips, where the probability of head is p = 0.5, to create a `binomial` random variable. We then repeated this a large number of times (N = 10,000), then plotted a histogram of the distribution of all binomial random variables. And because we did it so many times, it approximates the standard normal distribution (smooth bell shape curve). 

Just to demonstrate how this works, we can generate several `binomial` random variables:

![several_binomial](./several_binomial.png)

If we do this 10,000 times, we'll generate the above histogram. You'll notice that because we are testing whether the coin is fair, the probability of heads (success) *should* be at 0.5 and, from 1,000 coin flips, the **mean**(`mu`) should be a 500. 

We have another function that can help us calculate `normal_approximation_to_binomial`:

```python
import random
from typing import Tuple
import math


def normal_approximation_to_binomial(n: int, p: float) -> Tuple[float, float]:
    """Return mu and sigma corresponding to a Binomial(n, p)"""
    mu = p * n
    sigma = math.sqrt(p * (1 - p) * n)
    return mu, sigma
    
# call function
# (500.0, 15.811388300841896)
normal_approximation_to_binomial(1000, 0.5)
```
When calling the function with our parameters, we get a mean `mu` of 500 (from 1000 coin flips) and a standard deviation `sigma` of 15.8114. Which means that 68% of the time, the binomial random variable will be 500 +/- 15.8114 and 95% of the time it'll be 500 +/- 31.6228 (see [68-95-99.7 rule](https://en.wikipedia.org/wiki/68%E2%80%9395%E2%80%9399.7_rule)) 

## Hypothesis_Testing

Now that we have seen the results of our "coin fairness" experiment plotted on a binomial distribution (approximately normal), we will be, for the purpose of testing our hypothesis, be interested in the probability of its realized value (binomial random variable) lies **within or outside a particular interval**.

This means we'll be interested in questions like:

- What's the probability that the binomial(n,p) is below a threshold?
- Above a threshold?
- Between an interval?
- Outside an interval? 

First, the `normal_cdf` (normal cummulative distribution function), which we learned in a [previous post](https://paulapivat.com/post/dsfs_6/#distributions), *is* the probability of a variable being *below* a certain threshold. 

```python
normal_probability_below = normal_cdf

# probability that binomal random variable, mu = 500, sigma = 15.8113, is below 490

# 0.26354347477247553
normal_probability_below(490, 500, 15.8113)
```

On the other hand, the `normal_probability_above` would be 1 - 0.2635 = 0.7365:

```python
def normal_probability_above(lo: float,
                             mu: float = 0,
                             sigma: float = 1) -> float:
    """The probability that an N(mu, sigma) is greater than lo."""
    return 1 - normal_cdf(lo, mu, sigma)
    
# 0.7364565252275245
normal_probability_above(490, 500, 15.8113)
```

To make sense of this we need to recall the binomal distribution, that approximates the normal distribution, but we'll draw a vertical line at 490.

![binomial_vline](./binomial_vline.png)

We're asking, given the binomal distribution with `mu` 500 and `sigma` at 15.8113, what is the probability that a binomal random variable falls below the threshold (left of the line); the answer is approximately 26% and correspondingly falling above the threshold (right of the line), is approximately 74%. 

### Between interval

We may also wonder what the probability of a binomial random variable **falling between 490 and 520**:

![binomial_2_vline](./binomial_2_vline.png)

Here is the function to calculate this probability and it comes out to approximately 63%. *note*: Bear in mind the full area under the curve is 1.0 or 100%. 

```python
def normal_probability_between(lo: float,
                               hi: float,
                               mu: float = 0,
                               sigma: float = 1) -> float:
    """The probability that an N(mu, sigma) is between lo and hi."""
    return normal_cdf(hi, mu, sigma) - normal_cdf(lo, mu, sigma)

# 0.6335061861416337
normal_probability_between(490, 520, 500, 15.8113)
```
Finally, the area outside of the interval should be 1 - 0.6335 = 0.3665:

```python
def normal_probability_outside(lo: float,
                               hi: float,
                               mu: float = 0,
                               sigma: float = 1) -> float:
    """The probability that an N(mu, sigma) is not between lo and hi."""
    return 1 - normal_probability_between(lo, hi, mu, sigma)
    
# 0.3664938138583663
normal_probability_outside(490, 520, 500, 15.8113)
```



For more content on data science, machine learning, R, Python, SQL and more, [find me on Twitter](https://twitter.com/paulapivat).