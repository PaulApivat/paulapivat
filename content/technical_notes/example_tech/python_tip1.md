---
date: "2020-09-04T00:00:00+01:00"
draft: false
linktitle: Python Setup
menu:
  example_tech:
    parent: Python Tips 
    weight: 1
title: Python Setup Options
toc: true
type: docs
weight: 1
---

## Setting up Python for R Users

I've recently started #66DaysOfData and will be using this opportunity to make some headway into the world of Python. It's reputation for having a complex, at times frustrating, setup process precedes itself and is probably warranted. That said, here are some tips to minimize that. 

Here's my current OS environment. Mac users will have an older version of Python that comes with the computer, you can type `python --version` into your terminal to find out. Here's mine:

```
macOS Catalina version 10.15.5
Python 2.7.16
```

# Python 2 vs Python 3

There appears to be general consensus for anyone starting out in Python that you'll want Python 3. There's no debate here. Just get Python 3. I found the easiest way to go to Python Release for Mac OS X, which as of this writing is Python 3.8.5 and use the [macOS 64-bit installer](https://www.python.org/downloads/release/python-385/). 

Once installed, you'll want to check.

Instead of `python --version`, which checks Python 2, you'll use `python3 --version`. This implies that Python 3 isn't merely a "newer" version of Python, but that they are completely different categories. 

# Anaconda

While this isn't my first choice of development environment, it is the first option that allowed me to get coding in Python the fastest. 

You'll download the [Individual Edition](https://www.anaconda.com/products/individual) of the Anaconda, open-source platform. You'll download the application for your desktop and you'll find `Anaconda-Navigator` in your list of applications (or where ever you chose to place your newly installed application).

NOTE: Shortly after installing and using, the Desktop version of Anaconda froze and I had a difficult time even "Force Quitting" it, so my preferred method of launching Anaconda Navigator is to open the mac terminal and type in the command `anaconda-navigator`. 

The navigator supports `Jupyter Notebooks`, `PyCharm` and even `RStudio` among other environments. 

I will be using `Jupyter Notebooks` while I get acclimated to Python, but ultimately i'm looking for interoperability with #Rstats. 

# Reticulate

This is an `R package` that allows you to run `Python` code in `R` environments. The feature I am looking forward to using is the `R Markdown` document that allows me to run chunks of python code. 

Work-in-Progress: TBD

# VSCode

This is another popular IDE with widely used Python Extension. 

Work-in-Progress: TBD

# PyCharm

I've heard this IDE most closely resembles RStudio in ease of use.

Work-in-Progress: TBD

# Spyder

This appears to be close approximation of the functionality in RStudio. 

Work-in-Progress: TBD
