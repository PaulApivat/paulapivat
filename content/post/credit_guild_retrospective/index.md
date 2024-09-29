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
summary: 
tags: ["DeFi", "Lending Protocol", "Ethereum", "Credit Guild", "Credit System", "CDP"]
title: Credit Guild: WETH Market
---

**Reflection on the Ethereum Credit Guild**

Recently, the Ethereum Credit Guild announced its wind down. You can read the details of the announcement [here](https://x.com/OneTrueKirk/status/1833926962676707820). A few months ago, I collaborated with [Kirk](https://x.com/OneTrueKirk) and the Credit Guild team to create a dashboard to demonstrate user adoption. Unfortunately the project did not gain the necessary traction to continue. I'm using this article to open source some of the work and share my observation as a user and analyst. 

**Project Overview**

The **Ethereum Credit Guild** was intended to be a savings and credit system with loan terms that did not rely on external oracles. In the early days, the primary markets were WETH, USDC and ARB. I developed a dashboard primarily for the WETH market, planning to expand to other markets (i.e. USDC, wstETH, eUSD) once we solidified the necessary metrics. The goal was to secure an Arbitrum grant by demonstrating the usage and growth of the Credit Guild.

**Dashboard Features**

The [dashboard](https://dune.com/paulapivat/credit-guild-weth-market) highlighted active lending terms in the WETH market, showcasing deposits, collateral values, and borrowed amounts. Each market contained various terms delineating acceptable collateral types. 

Key metrics included:

- **TVL by Market:** Total collateral deposit values and liquidity in the Peg Stability Module (PSM) for the WETH market.
- **Debt by Market:** The volume of loans taken out relative to collateral.
- **Collateral Values by Term:** Aggregated collateral values for each term, indicating popular collateral types among depositors.
- **Lenders in Each Market:** A comprehensive list of depositors across different terms.
- **Average Metrics:** Including deposit growth, loan duration, and interest rates per term.
- **Debt Ceiling:** The maximum allowable debt issued against the pooled collateral.

**Unique Aspects of the Credit Guild**

The Credit Guild was unique in onboarding collateral types that had not been accepted by other lending protocols presumably as part of the protocol's strategy to compete with established players. However, tracking prices for these tokens posed a challenge since Dune Analytics did not list them. For example, Pendle's Principal Token (PT) was one such asset. Fortunately, I was able to adapt the pricing methodology developed by [HenryStats.eth](https://x.com/HenryBitmodeth), a member of the [Bytexplorer community](https://x.com/cryptodatabytes) (thanks Henry)!

**Analyst Insights**

With the "points meta" in full swing and I had deposited some EtherFi weETH tokens in Pendle. The Guild not only offered extra yield on my Pendle Principal Tokens but also provided a unique user experience that required active participation—from choosing terms and setting collateral ratios to staking GUILD or WETH tokens to mitigate default risks.

**User Interface Experience**

The platform demanded a hands-on approach, potentially daunting for users preferring a more streamlined experience. Despite its complexity, the experience was rewarding for those willing to engage deeply with the system.

**Looking Ahead**

I am eager to see what Kirk undertakes next and look forward to his continued insights into finance and banking. Check out his writings [here](https://onetruekirk.github.io/).


I'm always down to talk data, [shoot me a DM](https://twitter.com/paulapivat).