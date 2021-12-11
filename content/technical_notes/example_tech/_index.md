---
date: "2018-09-09T00:00:00Z"
draft: false
lastmod: "2018-09-09T00:00:00Z"
linktitle: Technical Note Example 
menu:
  example_tech:
    name: Technical Notes Overview
    weight: 1
summary: Learn how to use Academic's docs layout for publishing technical notes and tutorials.
title: Technical Notes Overview
toc: true
type: docs
weight: 1
---

## Technical Notes Table of Contents

- [Data Cleaning Tips](https://paulapivat.com/technical_notes/example_tech/data_cleaning_tip1/)
- [Data Viz Tips](https://paulapivat.com/technical_notes/example_tech/rstats_viz_2_y_axes/)
- [Database](https://paulapivat.com/technical_notes/example_tech/database_design_tips/)
- [Git & GitHub](https://paulapivat.com/technical_notes/example_tech/github_make_repo/)
- [Google Cloud Tips](https://paulapivat.com/technical_notes/example_tech/google_cloud_tip1/)
- [GraphQL](https://paulapivat.com/technical_notes/example_tech/graphql_comparison_operators/)
- [MongoDB](https://paulapivat.com/technical_notes/example_tech/mongodb_crud/)
- [Pandas](https://paulapivat.com/technical_notes/example_tech/pandas_pick_df_rows_columns/)
- [Pipeline](https://paulapivat.com/technical_notes/example_tech/pipeline_prep_index_before_insert_to_db/)
- [PostgreSQL](https://paulapivat.com/technical_notes/example_tech/postgresql_create_table/)
- [Python Tips](https://paulapivat.com/technical_notes/example_tech/python_tip2/)
- [Rstats Tips](https://paulapivat.com/technical_notes/example_tech/rstats_tip4/)
- [SQL](https://paulapivat.com/technical_notes/example_tech/sql_check_equality_two_columns/)
- [Webdev](https://paulapivat.com/technical_notes/example_tech/webdev_linting_error/)

## Flexibility

Add Technical Notes.

This feature can be used for publishing content such as:

* **Online courses**
* **Project or software documentation**
* **Tutorials**

The `courses` folder may be renamed. For example, we can rename it to `docs` for software/project documentation or `tutorials` for creating an online course.

## Delete tutorials

**To remove these pages, delete the `courses` folder and see below to delete the associated menu link.**

## Update site menu

After renaming or deleting the `courses` folder, you may wish to update any `[[main]]` menu links to it by editing your menu configuration at `config/_default/menus.toml`.

For example, if you delete this folder, you can remove the following from your menu configuration:

```toml
[[main]]
  name = "Courses"
  url = "courses/"
  weight = 50
```

Or, if you are creating a software documentation site, you can rename the `courses` folder to `docs` and update the associated *Courses* menu configuration to:

```toml
[[main]]
  name = "Docs"
  url = "docs/"
  weight = 50
```

## Update the docs menu

If you use the *docs* layout, note that the name of the menu in the front matter should be in the form `[menu.X]` where `X` is the folder name. Hence, if you rename the `courses/example/` folder, you should also rename the menu definitions in the front matter of files within `courses/example/` from `[menu.example]` to `[menu.<NewFolderName>]`.
