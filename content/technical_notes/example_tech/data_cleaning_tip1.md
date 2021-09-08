---
date: "2021-09-08T00:00:00+01:00"
draft: false
linktitle: Data Cleaning
menu:
  example_tech:
    parent: Data Cleaning Tips
    weight: 2
title: Survey Data Cleaning
toc: true
type: docs
weight: 2
---

## Survey Data Cleaning

**NOTE**: These tips are a distillation of data cleaning techniques I picked up in the course of cleaning data for the first DAO Creators Survey (Gitcoin x BanklessDAO).

The DAO Creators Survey was a two part survey sampling 442 and 256 respondents to approximately 50 survey questions ranging from demographics to web3 tooling, DAO compensation/healthcare and income stability, to name a few.

The questions ranged from highly structured (i.e., multiple choice, multiple response options and dropdown boxes) to highly unstructured (i.e., qualitative responses). 

I created approximately 50 charts for this report and each chart presented unique data cleaning challenges. However, I will describe a base foundation and areas of overlap so the next project is easier. 

Two articles were used for reference, but because this project optimized for speed, I did *not* do a full text analysis. 

1. [How to Generate Word Clouds in R](https://towardsdatascience.com/create-a-word-cloud-with-r-bde3e7422e8a)
2. [Text Mining with R: A Tidy Approach](https://www.tidytextmining.com/tidytext.html#summary)

### Pre-Cleaning Steps

The first move for any survey is to **change column names** into more manageable short codes and then **delete identifying information** to preserve privacy, for example:

```
df1 <- df %>%
    # Rename: shorten column names to be manageable
    rename(
       timestamp = "Timestamp",
       daos_work_for = "what DAO(s) do you work for? for each DAO, how many hours/month do you work? (feel free to include multiple)",
       city = "what city are you based in?",
       twitter = "whats your twitter username?",
       eth_addr = "whats your ETH address?",
       ) %>%
    # delete identifying information
    select(-twitter, -eth_addr, -email)
```

### Baseline Step: Convert Text to Tidy Format

This requires the `tidytext` package and a couple functions. The flow is to use `unnest_tokens()` to separate a string of words into a vector of individual words. Then follow-up with `anti_join()` to get rid of **stop words** (a corpus of words is provided with tidytext). 

Then, group and tally, which can be achieved with `group_by()` and `tally(sort = TRUE)` or one function `count(, sort = TRUE)`.

```
daos_work_tbl %>%
    unnest_tokens(word, text) %>%
    anti_join(stop_words) %>% 
    view()

```
If there are too many words, we can `filter()` and drop NA responses. With `dplyr` these operations can be chained to `ggplot2` to visualize the output. 

```
daos_work_tbl %>%
    unnest_tokens(word, text) %>%
    anti_join(stop_words) %>% 
    count(word, sort = TRUE) %>%
    filter(n > 3) %>%
    drop_na() %>%
```

### String Detect

Sometimes you need to use `str_detect()` to see how many instances of a string are present in a column. If there is a *match* of string detected, you want to categorize survey responses. This is structured combining `if_else()` conditionals with `str_detect()`.

This first requires creating an empty column:

```
# create empty column
daos_work_long$bin <- NA

# use if_else and str_detect
daos_work_long$bin <- if_else((str_detect(daos_work_long$word, "cre8")==TRUE), "cre8rdao", "NA")
daos_work_long$bin <- if_else((str_detect(daos_work_long$word, "mstable")==TRUE), "mstable", daos_work_long$bin)
daos_work_long$bin <- if_else((str_detect(daos_work_long$word, "marrow")==TRUE), "marrow dao", daos_work_long$bin)
daos_work_long$bin <- if_else((str_detect(daos_work_long$word, "badger")==TRUE), "badger dao", daos_work_long$bin)
daos_work_long$bin <- if_else((str_detect(daos_work_long$word, "raid")==TRUE), "raid guild", daos_work_long$bin)
daos_work_long$bin <- if_else((str_detect(daos_work_long$word, "metagame")==TRUE), "metagame", daos_work_long$bin)
```

### String Match

In some situations, you may want to see if a string *contains* a specific word. The function to use here is `str_match()`. Here, we're seeing if a string contains either `yes` or `yeah` or `no` or `not`:

```
income_stability_tbl2 <- income_stability_tbl %>%
    mutate(phrase = strsplit(as.character(text), ",")) %>%
    unnest(phrase) %>%
    count(phrase, sort = TRUE) %>%
    mutate(
        phrase_no = str_match(phrase, "[Nn]o|[Nn]ot")[,1],
        phrase_no = str_to_lower(phrase_no)
    ) %>%
    mutate(
        phrase_yes = str_match(phrase, "[Yy]es|[Yy]eah")[,1],
        phrase_yes = str_to_lower(phrase_yes)
    )

```

### Handling each survey question (column) separately

This requires splitting each column off. You could turn it into a `vector` first, then `tibble` or just subset a dataframe:

```
comp_denom_v <- as.vector(df1$comp_denom)
comp_denom_tbl <- tibble(line = 1:445, text = comp_denom_v)
```

### Manually add numbers 

Surprisingly, it was not easy to add items from the **same** category:

| Item  | Number |
|-------|--------|
| Zebra | 8      |
| Zebra | 17     |

It should be more straight forward to add Zebra. But instead we have to really manually add. For example, here i'm manually changing the `n` for bankless dao to `35`:

```
# bankless dao = 35
daos_work_long2$n[12] <- 35

```

### Delete specific rows

There are two ways to delete rows. First is to subset (a base R operation):

```
daos_work_long3 <- daos_work_long2[-c(4, 6, 13, 14, 15, 19, 24, 28, 31, 42, 46, 51, 52, 53,
                  61, 63, 67, 68, 69, 70, 78, 81, 87, 95, 100, 103),] %>% 
                  arrange(desc(n))
```

The second way is to use `slice` in `dplyr`. `Slice` can be used to select or re-order rows as well:

```
usd_earning_tbl3 <- usd_earning_tbl2 %>%
                    slice(4, 6, 7, 1:3, 5, 8:9)
```

### Assigning Factors to Preserve Order for Visualization

After using `slice` to re-order rows, we can use `mutate()` and `as_factor()` to create factors for visualization. This preserves the order we want (e.g., age range on the x-axis):

```
# reorder rows, save as new df
usd_earning_tbl3 <- usd_earning_tbl2 %>%
    slice(4, 6, 7, 1:3, 5, 8:9)

# need to sort by factors before visualize
usd_earning_tbl3 %>% 
    mutate(text_factor = as_factor(text))
```

### Separate String at Comma

Sometimes, simply turning a string into `tidytext` doesn't work because meaning phrases of two or three words *inadvertently* get split, so we may need to split by comma with `mutate()` and `strsplit()`, in lieu of using `unnest_tokens()`, then group and tally:

```
task_tabl2 <- task_tbl %>%
    mutate(phrase = strsplit(as.character(text), ",")) %>%
    unnest(phrase) %>%
    count(phrase, sort = TRUE) %>%
    view()
```

### Github Repo

See data cleaning scripts [here](https://github.com/PaulApivat/banklessDAO/tree/main/dao_survey_gitcoin)
