---
date: "2021-10-08T00:00:00+01:00"
draft: false
linktitle: 2 y-axes
menu:
  example_tech:
    parent: Data Viz Tips
    weight: 2
title: Two Y-Axes
toc: true
type: docs
weight: 2
---

## Creating 2 Y-Axes in a Plot

**Situation**: You want to create **two y-axes**. The left y-axis measures an amount, while the *right* y-axis converts the amount to a percentage. 

**Context**: This may be useful when you have a bar chart depicting relative amounts (e.g., membership numbers) and you require *another* y-axis (right side) to map *another* amount as a percentage of the former (e.g., % of members who successfully activated on their first day). 

This is exampe is taken from the [Bankless DAO Community Growth metrics](https://docs.google.com/presentation/d/18DGuSTsLgU2C2iNNcvoo2-27uL2Tcb1jfMaMGa9Zeyc/edit#slide=id.gf1c26dd130_1_11) where we looked at:

> How many new members successfully activate on their first day?

First, unlike typical use cases, we are **not** using `pivot_longer` in this example, but instead keep the data in a *wide* format (`pivot_wider`). We are visualizing 3 separate variables (columns) here, including: `new_members`, `pct_communicated` and `pct_opened_channels`. 

In addition, we'll have a hard-coded fourth line that represents the industry "benchmark".

*Because* we are not using a `pivot_longer` function, we will have to *manually* create our own legend. This will be explained in the commented code below:

```{python}
first_activation %>%
    rename(
        time = "interval_start_timestamp",
        new = "new_members",
        talked = "pct_communicated",
        visited = "pct_opened_channels"
    ) %>%
    ggplot(aes(x = time)) +
    # note: 3 separate charts: one bar and two lines
    geom_bar(aes(y = new, color = "New members"), stat = "identity", fill = "black") +
    # The left y-axis goes from 0-1500, 'talked' and 'visited' is multiplied by 1500/100 = 15
    # color is set to a string so it can be repurposed in the manually created legend
    geom_line(aes(y = talked*15, color = "% talked (voice or text)"), size = 1.5) +
    geom_line(aes(y = visited*15, color = "% visited more than 3 channels"), size = 1.5) +
    geom_line(y = 480, color = "orange", size = 0.2) +
    # scale_y_continuous(sec.axis) is the key to having two y-axes
    # The left y-axis goes from 0-1500, so sec.axis has ~./1500*100
    # name indicates both left and right y-axis label
    scale_y_continuous(
        name = "New Members",
        sec.axis = sec_axis(trans = ~./1500*100, name = "% Activated")
    ) +
    # We have to manually set color because we didn't actually set color in geom_line above
    scale_color_manual(values = c("white", "red", "black")) +
    # setting the color = "Legend" allows us to indicate on the chart where legend is
    labs(
        title = "How many new members successfully activate on their first day?",
        x = "",
        color = "Legend"
    )

```
### Converting Second Y-axis to another number aside from Percentage (%)

This is similar to the example above, except the math is adjusted so that if you wanted to convert **two scales** (and neither is a %). This is appropriate for when visualizing both a *total amount* (Messages sent) and *average amount* (Messages per communicator).

```{python}

msg_avg %>%
    rename(
        time = "interval_start_timestamp",
        per = "messages_per_communicator"
    ) %>%
    ggplot(aes(x = time)) +
    geom_bar(aes(y = messages, color = "Messages sent"), stat = "identity", fill = "black") +
    # conversion between two y-axis (similar to % conversion)
    # Here we convert 6000 to 12
    geom_line(aes(y = per*6000/12, color = "Messages per communicator"), size = 1.5) +
    geom_line(y = 5000, color = "orange", size = 0.2) +
    scale_y_continuous(
        name = "Messages sent",
        sec.axis = sec_axis(trans = ~./6000*12, name = "Message per ommunicator")
    ) +
    scale_color_manual(values = c("red", "black"))


```



For more content on data science, R, and Python [find me on Twitter](https://twitter.com/paulapivat).

