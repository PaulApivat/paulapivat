---
date: "2020-09-05T00:00:00+01:00"
draft: false
linktitle: Connect BigQuery to Data Studio
menu:
  example_tech:
    parent: Google Cloud Tips 
    weight: 1
title: Connecting BigQuery to Google Data Studio [Basic Setup]
toc: true
type: docs
weight: 1
---

## Steps for Connecting BigQuery to Data Studio 

This note outlines the basic steps required to generate charts in Google Data Studio, specifically pulling data from BigQuery.

# BigQuery

1. The starting point is to generate a query in BigQuery 
2. Once a query is created, click **Save Results**
3. In the pop-up window, a prompt: "choose where to save the results data from the query", save result as BigQuery Table
4. Set project name (i.e., jobsbot)
5. Set dataset name (i.e., internalmongo)
6. Create table name, for the specific query (i.e., jobfieldname_ranking)

# Google Data Studio

7. Click Add Data
8. Find BigQuery in Google Connectors
9. Locate saved query table (see above) (i.e., My Projects > jobsbot (project) > internalmongo (dataset) > jobfieldname_ranking (table/specific query))
10. Click Add 
11. Select 'Add a Chart' (note: could be Table or Chart style)
12. Optional: copy/paste Table to create a companion Chart for table
13. Select Table; in Data Menu, select Metric, 'Add Metric' to swap out generic default Report Count (for more informative data generated from the query)
