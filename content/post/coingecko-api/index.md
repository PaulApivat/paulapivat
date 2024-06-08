---
authors:
- admin
categories: []
date: "2024-06-08T00:00:00Z"
draft: false
featured: false
image:
  caption: ""
  focal_point: ""
lastMod: "2024-06-08T00:00:00Z"
projects: []
subtitle: Modeling data to build pipelines
summary: Modeling, then pulling, API data
tags: ["Python", "API", "data modeling", "pipelines", "coingecko"]
title: Exploring CoinGecko's API
---

From a data analyst perspective, one major difference between Web2 and Web3 is the existence of "public" data infrastructure. In crypto, analysts are familiar with [Dune Analytics](https://dune.com/) or [FlipSide Crypto](https://flipsidecrypto.xyz/) among [other data providers](https://www.primodata.org/blockchain-data). 

Teams of data engineers, database and backend specialists have done the heavy lifting so analysts can simply "use SQL" right in the browser. This makes the [data workflow in crypto](https://read.cryptodatabytes.com/p/2022-guide-to-web3-data-thinking) unique (and arguably better).

Nevertheless, if you work for a startup that happens to be building data-intensive applications, you might have to pull data from many sources. This post will explore [CoinGecko's API (demo tier)](https://www.coingecko.com/en/api) and describe a process of exploring the data, building an initial model as well as laying the foundation for building data pipelines. 

> Analysts are like restaurant patrons, with forks and knives out, ready to dine. This post will guide you to the kitchen to see how that food is prepared. 








I'm always down to talk onchain data, [shoot me a DM](https://twitter.com/paulapivat).