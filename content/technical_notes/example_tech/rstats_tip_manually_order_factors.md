---
date: "2021-10-08T00:00:00+01:00"
draft: false
linktitle: Manually Order Factors
menu:
  example_tech:
    parent: Rstats Tip
    weight: 2
title: Manually Ordering Factors
toc: true
type: docs
weight: 2
---

## Manually Ordering Factors

**Situation**: Sometimes you have want to display bar charts in a specific order, but the numbers get re-arranged or `group_by` and `sort` orders by value, but you have a specific order in mind.

**Context**: In this example, I have "Proposals" numbered "1-10". I want to display them in order, but number 10 doesn't go after 9, but instead goes 1, 10, 2, 3, and so on...I need to **manually set the factor order** so the bar chart displays exactly how I'd like.

I used `fct_relevel` (factor re-level), choose a specific column, and the second parameter is a vector of values with the factors manually arranged according to how I'd like. Then I set it to that specific column so R knows the desired factor level.

```{python}
# manually arranging factors - num
# note: move "proposal 10" to the end
# them apply newly arranged factor levels to column of interest
new$num <- fct_relevel(new$num, c("proposal 1", "proposal 2", "proposal 3", "proposal 4", "proposal 5", 
                       "proposal 6", "proposal 7", "proposal 8", "proposal 9", "proposal 10"))
```

I can also achieve this for string values that don't have an obvious order. Here i'm ordering the proposals by a known sequence (but not obvious to the audience).

```{python}
# manually arrange factors - name
# then apply newly arranged factor levels to column of interest
new$name <- fct_relevel(new$name, c("Approve the Bankless DAO Genesis Proposal",
                        "What charity should CMS Holdings donate 100k towards",
                        "Badge Distribution for Second Airdrop",
                        "Reward Season 0 Active Members",
                        "Bankless DAO Season 1",
                        "BanklessDAO Season 1 Grants Committee Ratification",
                        "BED Index Logo Contest",
                        "Request for funds for Notion's ongoing subscription",
                        "Transfer ownership of the treasury multisig wallet from the genesis team to the DAO",
                        "Bankless DAO Season 2"))
```

For more content on data science, R, and Python [find me on Twitter](https://twitter.com/paulapivat).