---
authors:
- admin
categories: []
date: "2024-09-29T00:00:00Z"
draft: false
featured: false
image:
  caption: ""
  focal_point: ""
lastMod: "2024-09-29T00:00:00Z"
projects: []
subtitle: A brief retrospective
summary: A brief retrospective
tags: ["Ethereum", "DeFi", "lending protocol", "dune analytics", "CDP"]
title: Credit Guild
---


Recently, the Ethereum Credit Guild announced its wind down. You can read the details of the announcement [here](https://x.com/OneTrueKirk/status/1833926962676707820). A few months ago, I collaborated with [Kirk](https://x.com/OneTrueKirk) to create a dashboard for the Credit Guild. We wanted to demonstrate user adoption and growth. I'm using this post to open source the work and share some reflections on the Credit Guild.

### Project Overview

I developed a [dashboard](https://dune.com/paulapivat/credit-guild-weth-market) for the WETH market, with plans to onboard other markets once we solidified the necessary metrics. The goal was to secure an Arbitrum DAO grant by demonstrating the usage and growth of the Credit Guild.


### Dashboard Features

The [dashboard](https://dune.com/paulapivat/credit-guild-weth-market) highlighted **active lending terms in the WETH market**, showcasing deposits, collateral values, and borrowed amounts.

Key metrics included:

- **TVL by Market:** Total collateral deposit values and liquidity in the Peg Stability Module (PSM) for the WETH market.
- **Debt by Market:** The volume of loans taken out relative to collateral.
- **Collateral Values by Term:** Aggregated collateral values for each term, indicating popular collateral types among depositors.
- **Lenders in Each Market:** A comprehensive list of depositors across different terms.
- **Average Metrics:** Including deposit growth, loan duration, and interest rates per term.
- **Debt Ceiling:** The maximum allowable debt issued against the pooled collateral.

### Unique Aspects of the Credit Guild

The Credit Guild was distinctive in accepting Pendle Principal Tokens as collateral, which became highly popular due to the "points meta" and Pendle's prominence in early 2024. However, tracking prices for these tokens posed a challenge since Dune Analytics did not list them. Fortunately, I adapted a pricing methodology from [HenryStats](https://dune.com/Henrystats), a member of the [Bytexplorer community](https://x.com/cryptodatabytes), shoutout to HenryStats!

### Analyst Insights

Pulling data for the Credit Guild meant dealing with collateral assets not typically found on other lending protocols, aligning with the protocol’s strategy to compete with established players. The Guild not only offered extra yield on my Pendle Principal Tokens but also provided a unique user experience that required active participation—from choosing terms and setting collateral ratios to staking GUILD or WETH tokens to mitigate default risks.

### User Interface Experience

The platform demanded a hands-on approach, potentially daunting for users preferring a more streamlined experience. Despite its complexity, the experience was rewarding for those willing to engage deeply with the system.

### Looking Ahead

I am eager to see what [Kirk](https://x.com/OneTrueKirk) undertakes next and look forward to his continued insights into [finance and banking](https://onetruekirk.github.io/).



I'm always down to talk data, [shoot me a DM](https://twitter.com/paulapivat).