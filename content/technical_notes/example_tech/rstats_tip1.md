---
date: "2019-05-05T00:00:00+01:00"
draft: false
linktitle: Reproducibility
menu:
  example_tech:
    parent: Rstats Tip
    weight: 2
title: Make your work reproducible
toc: true
type: docs
weight: 2
---

Understanding reproducibility and the `set.seed()` function in `R` is best achieved through generating various random numbers. Here are some more tips for making your work reproducible:

## Using set.seed()

Example of reproducibility in fitting ML models using set.seed()

```
#First Line
set.seed(1234)   

#Second Line
model_05_rand_forest_ranger <- rand_forest(
    mode = "regression", mtry = 4, trees = 1000, min_n = 10
    ) %>%
    set_engine("ranger", splitrule = "extratrees", importance = "impurity") %>%
    fit(price ~ ., data = train_tbl %>% select(-id, -model, -model_tier))

#Third Line
model_05_rand_forest_ranger %>% calc_metrics(test_tbl)
```


## Random Numbers

Here are several ways to get random numbers. These examples are informed by the `R Cookbook`, see [here](http://www.cookbook-r.com/Numbers/Generating_random_numbers/#:~:text=For%20uniformly%20distributed%20(flat)%20random,is%20from%200%20to%201.&text=To%20generate%20numbers%20from%20a,the%20standard%20deviation%20is%201.)

```
# get one random number using runif() from base-R, stats package
# default 0 to 1
runif(1)

# get two random numbers
runif(2)

# get a vector of three random numbers
# increase range beyond the default, -10 to 110
runif(3, min = -10, max = 110)

# ensure three random numbers do *not* have decimals
# use floor() function to round down
floor(runif(3, min = -10, max = 110))

# sample() function does the same thing - using just one function
# replace parameter: should sampling be with or without replacement?
sample(-10:110, 3, replace = TRUE)

# Reproducibility
# use set.seed() before any of the aforementioned random number generators

set.seed(123)
sample(-10:110, 3, replace = FALSE)
```

## Random Numbers from a Normal Distribution


```
# Get five random numbers from a normal distribution
# Here the default is mean = 0, standard deviation = 1.
rnorm(5)

# Change mean and standard deviation away from default
rnorm(5, mean = 66, sd = 12)

# Ensure reproducibility with set.seed()
set.seed(123)
rnorm(5, mean = 66, sd = 12)

# Ensure normal distribution by setting sufficiently large number with rnorm()
# Ensure reproducibility
# Plot a histogram

set.seed(123)
x <- rnorm(500, mean = 66, sd = 12)
hist(x)
```

