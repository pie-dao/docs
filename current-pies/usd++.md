# USD++

![Screen Shot 2020-05-26 at 9.38.42 AM\|690x386](https://forum.piedao.org/uploads/default/original/1X/06ccc0210cd93aa36451e5fe3cc2d478d050d4fe.jpeg)

### Links

* [How to mint USD++](https://medium.com/piedao/how-to-mint-usd-70501ad3db4e)
* [Etherscan](https://etherscan.io/token/0x9a48bd0ec040ea4f1d3147c025cd4076a2e71e3e#readContract)
* [Pool](https://balancer.piedao.org/#/pool/0x1Ee383389c621C37Ee5Aa476F88413A815083c5D)
* [Forum Post](https://forum.piedao.org/t/usd-v2-the-stablecoin-basket/116)
* [Excel file](https://docs.google.com/spreadsheets/d/1mlfHlDHZxTRJ6xcOdOnlmIMJQ4OYcIeguKt1F_3gkdc/edit#gid=853732443)

## Description

USD++ it’s a stable coin pool where weights are determined by volatility to the peg, trust minimization, and market risk. The V2 is an [evolution of the previous proposal](https://forum.piedao.org/t/wip-usd-usds-on-ethereum-diversified/111) which used an equally weighed allocation.

## Motivation

There is a variety of different stable coin on the Ethereum Network, some of those are centralized and rely on banks and custodian services, others are algorithmically managed and trust minimized.

Some stable coins, particularly the decentralized one, are often inefficient in keeping the peg to the dollar because of the soft-peg to the dollar which means they sometimes trade at a premium or at a discount compared to the price of the dollar.

For instance, on 3/18/2020 sUSD was trading al low as $0.43, holding a single stablecoin can be risky in isolation.

![Stablecoin variation \|600x371](https://forum.piedao.org/uploads/default/original/1X/0b6e77573b21db90befcd653feae939066728d19.png)

The deviation can happen both sides, DAI for instance, has recently been trading at a premium on the DAI/USDC Coinbase market, sometimes up to 12% for a little while. [Chart](https://docs.google.com/spreadsheets/d/e/2PACX-1vTcxB-WcFEAy9BRmjwOCJnxngsZYM0Pk_Xj6Kd8u4Ea_8PpulOOtP9qHoqttKaliIbCyiZ9oWQt5IXd/pubhtml?gid=1352735450&single=true).

![Screen Shot 2020-05-27 at 10.39.18 AM\|690x291, 100%](https://forum.piedao.org/uploads/default/original/1X/6ce85bfe99377b72b5e37b7b006e4650788a1880.png)

## Weight function

The weight function of USD++ would take into consideration a number of different factors including the volatility compared to the peg of $1, trust minimization, and market risk.

In short:

* Greater the peg &gt; Greater the weight.
* Greater the trust minimization &gt; Greater the weight.
* Greater the market volume / market cap &gt; Greater the weight.

A combination of those different factors seems to resonate with the vast majority of the people, particularly for long term holding of a cash position.

The following section of these post presents a scoring system which aims to distill those factors in a way that can be used to determine the weights of USD++.

## The Scoring System

This section aims to start a discussion about a scoring system designed to be used for the weights function of Pies.

The PieDAO DeFi Scoring System aims to be a tool of investigation regarding different aspects of tokenized assets used within a Pie.

Specifically for USD++, the focus is on Volatility, Trust minimization and Market Risk Each of these factors is then expressed in percentage point in the final model which determines the weights of USD++. For now, it looks like this:

| Scoring |  |
| :--- | :--- |
| Inverse Volatility score | 0.2 |
| Market Risk score | 0.5 |
| Trust minimization score | 0.3 |

### Volatility

Volatility for stable coin matters, it's the main reason why the industry had to make them considering assets like Bitcoin are more volatile that any normal human being can be okay with. However, how much it matters is a function of how long you are going to hold the stable coin.

**Generally speaking, considering a very short holding timeframe both trust and volatility don’t matter.**

In that use case, using DAI or USDC does not make a difference, the only thing that matters is that the market for that coin is liquid enough for you to swap your positions, **for this reason, USDT is currently dominating the stable coin market**, even if low reputation in the industry.

![total stablecoin supply\|690x371, 75%](https://forum.piedao.org/uploads/default/original/1X/613d03a552ce99481cbd6d1d84056415ff843e6a.jpeg)

As [milkyway16 pointed out:](https://twitter.com/vallard14/status/1265272487669051399)

> If we assume the trust leads to a fixed amount of risk of the value becoming 0$ every day and the risk of the peg almost stops increasing after a certain time.

GREEN = Total Trust risk for Time BLUE = Expected Deviation from Peg for different periods ![EY8mnjKWAAImQzS\|679x500, 75%](https://forum.piedao.org/uploads/default/original/1X/5b41f839ffa82098de17de2281715a30e5b55fde.jpeg)

Medium holding timeframes is where the comparative importance of low volatility around peg vs trust \(security\) is the highest, as represented in the matrix below.

|  | Trust required | Low Vol to Peg |
| :--- | :--- | :--- |
| Short hold | Low | Low |
| Medium hold | High | High |
| Long hold | High | Mild |

The Volatility Weight function used in USD++ is designed for medium/long holding of a cash position, it leaves space for normal price fluctuation of a specif stable coin as long as the peg is re-established during a reasonable timeframe \(max 7days, considering to go for 3\).

**How does it work**

* Considered for each asset the $ distance from peg \(1$\) at daily closing, for Q1 2020
* Daily values are aggregated into x’ days MA \(1 to 7\) 
* Each MA concurs to the computation of its annualized 90d\_stdev
* The Volatility Score is calculated for each underlying asset as the ratio between the inverse of STDEV and the sum of the inverse of STDEV of all underlying assets.

For instance, assuming a starting allocation of USDC / DAI / sUSD which relies ONLY on the Inverse Volatility Score that would look like this:

| Token | Total |
| :--- | :--- |
| USDC | 86.96% |
| DAI | 9.35% |
| sUSD | 3.69% |

### Trust minimization

Medium / Long term holding does not only requires a tight peg, it also requires confidence that your money is not going to be seized or taken away from you. For these reasons, centralized solutions might suffer from censorship as governments are becoming more mindful about the role of such coins. The majority of stable coins are already [under scrutiny by the Financial Stability Board](https://cryptobriefing.com/central-banks-recommended-ban-stablecoins/), including popular ones as USDT, USDC, TUSD, PAX, and DAI. It’s reasonable to assume that centralized ones would be more likely to be stopped first. The less trust needed the better.

The Score Parameters proposed are very much open to discussion, I believe improving these number will be a continuous process across the PieDAO community.

The way it works is simple, specif statements are made about the system and a score is associated with that statement. If the statement holds true for a specif coin, points are assigned.

| **Description** | DAI | USDC | sUSD | Max Score |
| :--- | :--- | :--- | :--- | :--- |
| No single admin key | 1 | 1 | 1 | 1 |
| More than 2 people needed to upgrade. | 1 | 1 | 1 | 1 |
| Requires on-chain governance vote | 10 | 0 | 0 | 10 |
| No governance requires at all \(100% algo\) | 0 | 0 | 0 | 20 |
| No censorship function in smart contract | 20 | 0 | 20 | 20 |
| No emergency shutdown function | 0 | 0 | 1 | 1 |
| No KYC involved at any point | 5 | 0 | 5 | 5 |
| No geofencing \(front end\) | 1 | 1 | 1 | 1 |
| Has not been censored before | 10 | 10 | 10 | 10 |
| No legal attack vector | 0 | 0 | 0 | 5 |
| No economic attack vector | 0 | 5 | 0 | 5 |
| No oracle risk | 0 | 5 | 0 | 5 |
| No high severity events ever | 0 | 10 | 0 | 10 |
| No high severity events in the last 6M | 0 | 5 | 0 | 5 |
| **Total** | **48** | **38** | **39** | **99** |

A few notes on definitions:

The "legal attack vector" is represented by having a legal structure \(company/foundation\) which can be compelled to act by governments in combination with shutdown functions, upgradability, or whitelisting abilities.

The "High severity events" is represented by any events involving user's funds lost. The [MakerDAO Black Thursday](https://medium.com/@whiterabbit_hq/black-thursday-for-makerdao-8-32-million-was-liquidated-for-0-dai-36b83cac56b6) is considered as such, as well as the [Synthetix Oracle Incident](https://blog.synthetix.io/response-to-oracle-incident/).

Going back to our example before, assuming we would use only the Trust Minimization score for the allocation, it would look as such:

| Token | Total |
| :--- | :--- |
| USDC | 30.40% |
| DAI | 38.40% |
| sUSD | 31.20% |

DAI comes out with a smaller weight that some people might expect, as a matter of fact, without the recent Black Thursday it would have had the greater weight \(&gt;45%\).

### Market Score

For market score, I'm using the AAVE risk framework for the time being as I think it represents well the risk based on volume and market cap. If you didn't yet, [please learn more in their docs](https://docs.aave.com/risk/currency-risk/methodology).

Borrowing the AAVE score:

| Market risk \(AAve Methodology\)\* | DAI | USDC | sUSD | TUSD | USDT | BUSD | Score |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| MarketCAP | 6 | 9 | 1 | 7 | 10 | 7 | 12 |
| Liquidity | 6 | 9 | 1 | 8 | 12 | 6 | 12 |
| **Total** | **12** | **18** | **2** | **15** | **22** | **13** | **24** |

Back to the example before, assuming we would use only the Market Score for the allocation, it would look as such:

| Token | Total |
| :--- | :--- |
| USDC | 56.25% |
| DAI | 37.50% |
| sUSD | 6.25% |

## Conclusion

Below you'll find the final allocation by putting the 3 factors together. Quarterly update of the index expected for October 8, 2020.

| USDC | Circle/Coinbase | TRUE | 64.32% | 17.83% | 38.30% | 47.22% |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| DAI | MakerDAO | TRUE | 6.92% | 46.50% | 25.53% | 20.42% |
| TUSD | Trust Token | TRUE | 26.03% | 29.94% | 31.91% | 28.58% |
| sUSD | Synthetix | TRUE | 2.73% | 5.73% | 4.26% | 3.788% |

