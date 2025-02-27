# [Temp Check] Start Building a Curve DAO Treasury

# Summary

Curve's current "treasury" consists of $16m in CRV tokens. All protocol revenue is currently redistributed to LPs, veCRV and scrvUSD and there are no mechanism to grow the treasury. This is not sustainable in the middle to long term. Currently, the only way to cover the costs of maintaining and growing the protocol (payroll, audits, services, etc.) is by selling this limited amount of CRV. The amount would only cover ~3 years of expenses at the current estimated burn rate. This proposal introduces two mechanisms to redirect a part of pool fees towards a protocol treasury. 

# State of the DAO's Finances

## Current Assets and Annual Expenditures

**The DAO has ~$16m in assets and spends ~$5m per year. It currently only has three years of runway.**

Curve has no formal treasury to speak of. The closest equivalent is the [community fund](https://etherscan.io/address/0xe3997288987e6297ad550a69b31439504f513267), which was originally seeded with [151,515,151 CRV back in 2020](https://gov.curve.fi/t/scip-2-utilizing-the-community-fund/967). Later, 28 million CRV were allocated to the grant council ([here](https://curvemonitor.com/dao/proposal/ownership/30) and [here](https://etherscan.io/tx/0xd8162f097998153903c9a28c960de269deabae8b6497a7c9a3378b6f9e71c8ad)), 75.5 millions were spent to refund victims of the [re-entrancy hacks](https://hackmd.io/@vyperlang/HJUgNMhs2) ([here](https://curvemonitor.com/dao/proposal/ownership/521) and [here](https://curvemonitor.com/dao/proposal/ownership/593)), and 21 million went to [Swiss Stake](https://gov.curve.fi/t/funding-proposal-for-swiss-stake-ag-the-company-building-curve/10204) to cover upcoming development ([here](https://curvemonitor.com/dao/proposal/ownership/828)).

Of the original amount, 26 million CRV (USD $12m at current prices) remain in the community fund and 9 million (USD $4m) in the grants council multisig. The DAO needs to spend $4m per year to support development and audits via Swiss Stake. The costs of other service providers (such as Llama Risk) and grants bring the total to over $5m/y. In addition to paying developers and auditors, the protocol could also use treasury funds for a variety of other purposes (cover bad debt if it threatens the protocol's viability, incentivize new products, etc.). Being entirely denominated in CRV, the treasury is exposed to the price risk of the CRV token. Selling assets will also put downward pressure on the price of the token. 

While **the situation requires urgent action**, it is far from being critical as the protocol generates a healthy amount of revenue.

## DAO Revenue

**The DAO has generated over $40m in revenue in 2024 and is on track to reach a similar amount in 2025. Basic operating costs only represent ~10% of this amount.**

While Curve's revenue fluctuates with the fortunes of the crypto market, the protocol generated over $40m in fees in 2024. Fees for the first two months of 2025 amount to ~$5.4m, which, with a crude projection, should get us above 35m this year. A typical startup's operating expenses account for 60 to 90% of revenue, for Curve it is only about 10%.

![image](https://github.com/user-attachments/assets/2ca90228-01e9-41d5-8b39-d51a3bcc081b)


If the money is not used to cover the protocol's operating expenses, where does it all go? In short, veCRV. A baffling 70% of the DAO revenue is redirected towards governance token stakers:

![image](https://github.com/user-attachments/assets/356571d5-9065-49e5-8c71-3522aa3cacdb)

This capital could be strategically reinvested in product development, security infrastructure, and market expansion initiatives, rather than distributed to stakeholders who—due to the emergence of liquid wrappers—lack long-term incentives to steward the protocol's growth.
While these distributions provide some value, the current approach resembles a scenario where a four-year-old startup allocates 70% of its revenue to dividends—a practice inconsistent with standard growth-stage business models. For context, technology companies that offer dividends typically maintain a median payout ratio of approximately 15%, which is five times lower than the DAO's current distribution rate.
Additionally, it's important to recognize the evolving contributor landscape. Though early protocol contributors received substantial CRV allocations they could lock for yield generation, the composition of the core team has undergone almost complete turnover since inception. Consequently, most current contributors hold veCRV positions insufficient to generate sustainable income from yield alone.


This is capital that could be reinvested in development, security and marketing but instead goes to reward stakeholders who, since the rise of liquid wrappers, lack long-term incentives to steward the protocol's growth. That is not to say that veCRV provides no value to the protocol, but this situation is akin to a four-year-old startup allocating 70% of its revenue to dividends. 

Companies do not typically redistribute revenue in the early years of their lifecycle, and if they do, the [median dividend payout ratio](https://pages.stern.nyu.edu/~adamodar/New_Home_Page/datafile/divfund.html) for tech companies is around ~15%, 5 times less than what the DAO currently pays out. It's also worth noting that while early contributors to the protocol received a sizeable CRV allocation they could lock to receive yield, the composition of the core team and other contributors has almost entirely changed since the early days of the protocol. Almost none of the current contributors have a veCRV position sizeable enough to generate sustainable income from yield. 

# Treasury Accumulation Strategies

**The DAO can start accumulating a treasury by either taking a larger cut of the yield of certain DEX pools or a small cut of the distributed fees**. 

This has the advantage of diversifying the treasury as fees will be paid either in yield bearing LP tokens or in crvUSD.

## Strategy #1: Redirect Fees of Selected Target Pools, Starting with 3pool

### Overview

**Directing all fees from 3pool to the treasury via a new proxy owner contract would cover the majority of the protocol's expenses, but would rely entirely on a single pool and its largest LP.**

A [recent DAO proposal](https://curvemonitor.com/dao/proposal/parameter/90) raised the [3pool](https://etherscan.io/address/0xeCb456EA5365865EbAb8a2661B0c503410e9B347)'s admin fees to 100% meaning that all of its trading fees now redistributed to veCRV. The 3pool is one of the highest earning pools on Curve and its revenue alone would cover most of the protocol's operating expenses:

![image](https://github.com/user-attachments/assets/b34dcb56-d19b-4dd6-b163-0f03c165989d)

Recent changes to the pool's parameters ([higher A](https://curvemonitor.com/dao/proposal/parameter/90) and [higher trading fee](https://curvemonitor.com/dao/proposal/parameter/91)), are also projected to increase revenue. While the changes are too recent to draw any definitive conclusion, the preliminary data is encouraging:

![image](https://github.com/user-attachments/assets/79000df4-699c-4db8-8a09-eb02c145ecc9)


While a set of special circumstances make the 3pool an ideal candidate for this strategy, other pools could be targeted. For instance, to incentivize migration, the fees on an obsolete pool could likewise be increased 100% to the benefit of the DAO and redirected towards the treasury. Or future pools could be deployed that split the 50% admin fee share between veCRV and the treasury.

### Deployment

This strategy will require a contract akin to crvUSD's [FeeSplitter](https://github.com/curvefi/fee-splitter/blob/main/contracts/FeeSplitter.vy), but adapted to DEX pools and some of their peculiarities. The contract for 3pool for instance would need to implement some ownership admin functions. Newer factory pools present another set of challenge as the fee_receiver is shared among all pools, but the contract could likewise act to reallocate fees between veCRV and the treasury. 

For 3pool specifically, 2 proposals will be needed, one to `commit_transfer_ownership` and another to `apply_transfer_ownership`. 

### Assessment

| Pros | Cons |
|------|------|
| • Relatively painless for veCRV holders, as half of funding will come from revenue previously redistributed to LPs | • Risky and unsustainable: treasury will depend on fees from a single pool with one major LP rather than benefit from overall protocol growth |
| • Flexible: can redirect part of the fees on targeted individual pools or pool types | • If other pools are added after 3pool, will need to handle burning as fees will be paid in LP tokens |


## Strategy #2: Take a 10% cut of all DAO revenue


### Overview

**The cut would be applied directly on all revenue distributed to veCRV for a small decrease of 40bps in APR**

The DAO currently generates $40m in revenue and 10% of that would start to cover operational expenses. This strategy yields steady stablecoin revenue that would grow with the protocol's revenue. The cut would lower veCRV APR by an equivalent 10% which at current value is a small 40bps decrease. 

There is currently USD $438 million in the [veCRV contract](https://etherscan.io/address/0x5f3b5DfEb7B28CDbD7FAba78963EE202a494e2A2#code), and the last weekly fee distribution was $333,121. Discounting lock times, this gives an APR of 3.95% ($333,121 / $438,000,000 * 52). With a 10% lower fee distribution, the APR would have been 3.55% instead. As a significant of the yield for CRV now comes from bribes and additional token emissions or incentives from liquid staking protocols, this should not decrease CRV's attractiveness as a yield generating asset.


### Deployment

This strategy would likewise use a [FeeSplitter](https://github.com/curvefi/fee-splitter/blob/main/contracts/FeeSplitter.vy) like contract which could be added as a hook to the Fee Collector's [Hooker](https://etherscan.io/address/0x9a9df35cd8e88565694ca6ad5093c236c7f6f69d#code) contract. The contract would then take a portion of fees already converted to crvUSD and forward the remainder to the usual fee distribution contract for veCRV. Updating the hookers will require a single proposal/vote by the DAO.

### Assessment

| Pros | Cons |
|------|------|
| • Straightforward and easy to keep track of as there is one single point for splitting (as opposed to potentially multiple individual pools for strategy #1) | • Direct impact on incentives: each percentage point of fees redirected causes an equivalent percentage reduction in veCRV APR |
| • Protocol treasury grows proportionally to overall protocol revenue | • Less flexible as we can't target specific pools or products |


# Final Destination of Funds

**All funds taken from the protocol's revenue will be redirected to the [community fund](https://etherscan.io/address/0xe3997288987e6297ad550a69b31439504f513267).**

This has the following advantages:
- The DAO maintains control over the funds, and any allocation from the treasury will require DAO approval
- The community fund has hitherto acted as an all-purpose money stash to pay for development, grants and insurance at the DAO's discretion. This flexibility and vagueness of purpose is a desirable feature.


# What this proposal is NOT about

- This proposal is **NOT** meant to start a debate on treasury management (what assets to hold in the treasury, what yield strategies to use)
- This proposal is **NOT** meant to start a debate on how to allocate the funds (Swiss Stake, grant council, autobribes, etc.)

  
While these are potentially interesting discussion topics, they are only worth discussing if the DAO has money. The DAO currently has no money. Let's keep the discussion focused on ways the DAO can start making money.

