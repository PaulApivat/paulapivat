---
date: "2020-08-25T00:00:00+01:00"
draft: false
linktitle: Tidy Data - Spread
menu:
  example_tech:
    parent: Rstats Tip
    weight: 2
title: Use the spread() function
toc: true
type: docs
weight: 2
---

One principle of `tidy` data is to change from wide to long; and conversely, long to wide. 

Here's a concrete example:

## Using spread()

The first part of the below pre-processing steps include subsetting the original data frame (data) by selecting two countries for comparison (Morocco and Qatar) on specific variables such as: `GeoAreaName`, `TimePeriod`, `Sex`, `Type of skill` and `Value`. 

Then employing `group_by` to ensure all rows are unique. The next line is key as it addresses an error that each row must be marked by a unique id key.

Finally, the `spread()` function allows us to see each countries' relative performance on various [ICT skills](http://tcg.uis.unesco.org/4-4-1-proportion-of-youth-and-adults-with-information-and-communications-technology-ict-skills-by-type-of-skill/). Please consult meta-data for more details. 

```
data %>%
    filter(GeoAreaName=="Morocco" | GeoAreaName=="Qatar") %>% 
    select(GeoAreaName, TimePeriod, Sex, `Type of skill`, Value) %>%
    group_by(GeoAreaName, TimePeriod, Sex, `Type of skill`, Value) %>%
	
    # Error: Each row of output must be identified by a unique combination of keys.
    # rowid_to_column() address this error
	
    tibble::rowid_to_column() %>%
    spread(key = `Type of skill`, value = Value)

```