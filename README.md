# maker taker fees crypto: what they actually cost, why the gap exists, and how to pay less per trade

If you've ever checked your order history after a busy session and noticed the balance is lighter than you expected, trading fees are usually the reason. Every fill has two sides — the entry and the exit — and the maker/taker split quietly decides how much of your edge survives the round trip. The percentages look tiny, but they compound the way all percentages do: invisibly, then suddenly.

This guide breaks down what maker and taker fees actually are in crypto, how they're calculated, where they differ across exchanges, and which habits reliably push you toward the cheaper side of the schedule. We'll use OKX's current fee framework as the concrete reference point, because its tiered structure is one of the most transparent in the industry and illustrates every concept cleanly.

## What "maker" and "taker" actually mean

A **taker** order is one that executes immediately against an order already resting on the book. You send it, it fills now, it removes liquidity. A **maker** order is one that sits on the book at a set price and waits for a counterparty to fill it. You send it, it rests, it adds liquidity.

Exchanges reward the second behavior with a lower fee, because resting orders are what make a book deep. If every participant only sent market orders, spreads would widen and fills would get ugly. The maker/taker spread is, in effect, a tax on impatience — and the people willing to wait get the discount.

The nuance that costs beginners real money: **classification is based on execution, not order type.** If you submit a limit order priced aggressively enough to cross the spread the instant it arrives, it fills immediately and is billed as a taker order, even though you used the limit-order ticket. A market order is almost always a taker fill, but a limit order is not automatically a maker fill. The only reliable way to *guarantee* a maker classification is to flag the order **post-only**, which tells the matching engine to reject it outright rather than let it cross.

## The default maker and taker rates on a major exchange

Once you finish KYC and place your first trade on a major centralized exchange like OKX, you land in the regular-user tier. These are the headline rates that apply before any volume discount, token-holding benefit, or referral rebate:

| Product | Maker Fee | Taker Fee |
| --- | --- | --- |
| Spot | 0.080% | 0.100% |
| Margin (cross/isolated) | 0.080% | 0.100% |
| Perpetual swaps | 0.020% | 0.050% |
| Delivery futures | 0.020% | 0.050% |
| Options | 0.020% | 0.030% |

A few things jump out. Spot fees are roughly four to five times the derivatives fees at the same tier — a $10,000 market buy on spot costs about $10 in fees, while the same notional opened as a perpetual position costs around $5 per side. And the maker/taker gap is proportionally wider on perps: spot's maker is 80% of its taker, while perp's maker is only 40% of its taker. If your strategy can survive resting orders on derivatives, the relative reward for patience is much steeper.

> The maker/taker spread is the most underestimated line item in a trading account. On spot, every maker fill saves you 0.020 percentage points versus a taker fill. On perps, you save 0.030. Multiplied across a month of round trips, that funds a non-trivial slice of extra capital to trade with.

If you want to start on these rates with a permanent 20% rebate layered on top, you can 👉 [sign up on OKX with the CASH20 code](https://okx.com/join/CASH20) — the referral field pre-fills automatically and the rebate applies from your first trade.

## How the fee is actually calculated

The rates above are percentages. Here is what they do to a position.

**Spot and margin.** The fee is charged in the base currency and equals the fee rate multiplied by the amount of crypto bought or sold at fill. Suppose BTC sits at $20,000 and you market-buy 1 BTC as a taker at the regular-user rate of 0.10%. Your fee is:

$$0.0010 \times 1 = 0.001 \text{ BTC}$$

so you receive 0.999 BTC. If instead you sell 1 BTC via a resting limit order and become the maker at 0.08%, the fee is:

$$0.0008 \times 20{,}000 = 16 \text{ USDT}$$

leaving you with 19,984 USDT.

**USDT- and USDC-margined futures.** The formula is:

$$\text{Fee} = \text{Fee rate} \times (\text{Contracts} \times \text{Multiplier} \times \text{Contract size} \times \text{Fill price})$$

Take BTC-USDT perps with a 0.01 BTC contract size. You market 100 contracts (1 BTC) at $20,000 as a taker at 0.05%:

$$0.0005 \times (100 \times 1 \times 0.01 \times 20{,}000) = 10 \text{ USDT}$$

The same trade worked as a resting maker order at 0.02% costs:

$$0.0002 \times (100 \times 1 \times 0.01 \times 20{,}000) = 4 \text{ USDT}$$

Over a round trip, that's a $12 swing on a single $20,000 position — and it compounds fast across a month of trading.

**Crypto-margined futures.** The settlement currency flips, so the formula divides by price instead of multiplying:

$$\text{Fee} = \text{Fee rate} \times \left(\text{Contracts} \times \text{Multiplier} \times \frac{\text{Face value}}{\text{Fill price}}\right)$$

**Options.** The fee is the smaller of the rate-based calculation and 7% of the option premium, which prevents absurd fees on deep out-of-the-money contracts. Exercise fees and forced-liquidation fees follow parallel min-formula logic.

A couple of universal rules worth memorizing: forced liquidation is always billed at your current taker rate (so a blown position costs more than a planned one), delivery-futures settlement carries a flat 0.01% for every user regardless of tier, and spreads/combo trades get 50% off the classic order-book rate for each leg.

## Spot vs futures vs perps vs options: which costs less

The honest answer is "it depends on what you're trying to do," but the fee arithmetic has a clear shape.

For pure directional exposure, **perpetual swaps are the cheapest vehicle per unit of notional** — 0.02% maker / 0.05% taker at the regular tier, descending toward zero and then negative maker at the top VIP tiers. The catch is **funding**: perps charge a periodic funding rate paid between longs and shorts to keep the contract tethered to spot. Funding is not a maker/taker fee and is not paid to the exchange — it's paid trader-to-trader. But it is a real cost, and on a position held across multiple funding intervals it can easily exceed the trading fee. The realistic per-trade cost on a perp is round-trip fee plus funding pro-rated across the hours you hold.

**Delivery futures** share the perp fee schedule but replace funding with basis cost (the price drift toward expiry). **Options** start low and stay capped at 7% of premium, which makes them fee-efficient for defined-risk setups but introduces premium decay as the dominant cost. **Spot and margin** carry the highest headline fees but no funding and no liquidation mechanics (margin excepted), which makes them the cleanest vehicle for long-term holds and the most expensive for churning.

For one-off small swaps, **OKX Convert** is worth knowing about: it returns a single price with no separately quoted maker/taker fee, because the cost is baked into the spread. That's friendlier for tiny sizes and worse for anything large enough to hit a normal spot book without slippage. If you are moving size, the order book wins.

## The full tier schedule: how fees descend as volume grows

OKX runs a **volume-based VIP track** (Regular, then VIP 1 through VIP 9) on top of the base rates. Your tier is recalculated daily, and you always get the *highest* tier you qualify for across any single product line — so a big futures month lifts your spot and options rates too. The framework was updated in November 2025 to streamline spot pairs into three groups and futures pairs into two groups, and the futures thresholds were further adjusted in April 2026 to lower the entry bar for VIP 1–3.

### Spot fee schedule (Standard Group pairs)

| Tier | Assets (USD) or 30-day Volume (USD) | Maker | Taker |
| --- | --- | --- | --- |
| Regular user | < 100,000 / < 1,000,000 | 0.0800% | 0.1000% |
| VIP 1 | 100,000 / 1,000,000 | 0.0675% | 0.0800% |
| VIP 2 | 250,000 / 5,000,000 | 0.0600% | 0.0700% |
| VIP 3 | 500,000 / 10,000,000 | 0.0550% | 0.0650% |
| VIP 4 | 2,000,000 / 20,000,000 | 0.0300% | 0.0450% |
| VIP 5 | 5,000,000 / 100,000,000 | 0.0250% | 0.0350% |
| VIP 6 | 10,000,000 / 200,000,000 | 0.0000% | 0.0300% |
| VIP 7 | — / 500,000,000 | -0.0020% | 0.0250% |
| VIP 8 | — / 1,000,000,000 | -0.0050% | 0.0200% |
| VIP 9 | — / 5,000,000,000 | -0.0075% | 0.0175% |

### Futures fee schedule (post-April 2026 update, Group 1 top pairs)

| Tier | Assets (USD) or 30-day Futures Volume (USD) | Maker | Taker |
| --- | --- | --- | --- |
| Regular user | < 100,000 / < 5,000,000 | 0.0200% | 0.0500% |
| VIP 1 | 100,000 / 5,000,000 | 0.0160% | 0.0450% |
| VIP 2 | 200,000 / 10,000,000 | 0.0150% | 0.0360% |
| VIP 3 | 2,000,000 / 50,000,000 | 0.0100% | 0.0280% |
| VIP 4 | 5,000,000 / 200,000,000 | 0.0080% | 0.0270% |
| VIP 5 | 20,000,000 / 600,000,000 | 0.0050% | 0.0260% |
| VIP 6 | 50,000,000 / 1,000,000,000 | 0.0000% | 0.0250% |
| VIP 7 | 100,000,000 / 1,500,000,000 | -0.0020% | 0.0200% |
| VIP 8 | 250,000,000 / 2,000,000,000 | -0.0050% | 0.0200% |
| VIP 9 | 500,000,000 / 20,000,000,000 | -0.0050% | 0.0150% |

Read the negative numbers carefully — they are not a typo. From VIP 7 upward, the maker fee turns **negative**, which means the exchange *pays you* to post liquidity. That is the structural endpoint of every maker-taker model: at the top, the platform is buying depth from the people who provide it. The taker side never goes negative; someone always pays to jump the queue.

For full tier details and to start on these rates with a 20% rebate on top, you can 👉 [open an OKX account with the CASH20 referral code](https://okx.com/join/CASH20).

## The OKB track: a retail-friendly lever within the regular band

Not everyone can grind millions in monthly volume. OKX also runs an OKB-holding track within the regular-user band (Lv 1 through Lv 5). OKB is OKX's native platform token, and holding it moves you up within the regular-user band **without any 30-day volume requirement**. The first step is modest: holding 500 OKB lifts you from Lv 1 to Lv 2, with each subsequent level requiring a larger balance and unlocking a small but compounding discount on top of the base rate.

This is genuinely useful for casual traders, but with one honest caveat: buying OKB *purely* for the fee discount is rarely worth it once you account for OKB's own price swings. The math only works cleanly if you would have held OKB anyway as part of your portfolio. Treat the OKB discount as a bonus on top of a token you already want, not as a standalone fee-optimization trade.

## How OKX compares to other major exchanges

At the entry tier, the headline numbers across the big centralized exchanges are closer than newcomers expect — 0.10% taker is roughly the global default for spot. The differences sharpen at higher volumes and across product lines.

| Exchange | Spot Maker | Spot Taker | Futures Maker | Futures Taker |
| --- | --- | --- | --- | --- |
| **OKX (regular)** | **0.08%** | **0.10%** | **0.02%** | **0.05%** |
| Binance (base) | ~0.10% | ~0.10% | 0.02% | 0.05% |
| Bybit (base) | ~0.10% | ~0.10% | 0.01% | ~0.05% |
| Coinbase (Advanced) | 0.40% | 0.60% | — | — |
| Kraken | 0.16% | 0.26% | — | — |

OKX's spot maker of 0.08% is among the lowest entry-level maker rates in the industry, and its derivatives schedule is competitive with anyone. Where OKX pulls ahead in practice is the combination of deep liquidity on major pairs, a tiered framework that actually descends to *negative* maker fees at the top, and the ability to layer a referral rebate on top. Where it gets complicated is regional availability — the experience varies by jurisdiction, with an EEA-specific fee framework and a U.S. framework that introduced fee groups at the trading-pair level. Always check the fee page that loads for *your* region after login.

## The four levers that lower your fees

There are four levers, ranked roughly by how accessible they are to a normal trader.

**1. Work limit orders with the post-only flag.** This is the single highest-ROI habit and it costs nothing. Set your limit price one tick inside the spread, flag the order post-only, and wait the few extra seconds for a passive fill. Worst case the order is rejected and you re-submit; best case you captured the maker discount on every fill for free. Over a month of round trips this funds a meaningful slice of additional capital.

**2. Use the CASH20 referral rebate — 20% back, permanently.** When you sign up on OKX with the CASH20 code, you bind a referral relationship that returns **20% of your trading fees** to you on an ongoing basis. It is not a one-time bonus — it persists as long as the referral relationship is active, and it stacks on top of whatever VIP or OKB tier you happen to be in. On $100,000 of monthly spot volume at the taker rate, the base fee is about $100; the rebate puts $20 of that back in your pocket, every month, automatically. For anyone who plans to trade regularly, you can 👉 [claim the CASH20 20% fee rebate at signup](https://okx.com/join/CASH20) — the code can only be applied at registration, so it's worth binding before you deposit.

**3. Climb the OKB track (Lv 2–5).** Holding 500 OKB lifts you from Lv 1 to Lv 2 with no volume requirement, and larger balances unlock further discounts. Realistic for retail, but only worth it if you would hold OKB anyway.

**4. Climb the VIP track (VIP 1–9).** The serious lever, gated by 30-day volume and/or asset balance. The first jump from Lv 1 to VIP 1 now needs around $5 million in monthly futures volume or $100,000 in assets after the April 2026 threshold cut — still well above anything a casual trader hits in a year, but meaningful once you are running real size. At the very top, maker fees turn negative and the exchange pays you to provide liquidity.

One combination worth noting: the CASH20 20% rebate applies on top of your tier rate, so a VIP 3 futures maker paying 0.0100% effectively pays 0.0080% after rebate. The referral does not replace the tier discount — it sits on top of it.

## Common mistakes that quietly cost you the maker rate

Most fee leakage doesn't come from picking the wrong tier. It comes from accidentally turning would-be maker orders into taker orders. The usual culprits:

- **Aggressive limit pricing.** You want to buy and you set the limit at or above the best ask. It crosses instantly. Taker fee, even though you used the limit ticket.
- **Fast markets.** You place a limit buy just below the ask, but price ticks up before your order reaches the matching engine. The order is now marketable on arrival. Taker fee.
- **Stop-loss as market.** Stops trigger as market orders by default. Every triggered stop is a taker fill, on top of any slippage.
- **Take-profit as market.** Same logic.
- **IOC or FOC time-in-force.** Immediate-Or-Cancel and Fill-Or-Kill are designed to take liquidity. Taker fee, by construction.

The fix in every case is the same: when your idea has a window of minutes or hours rather than seconds, set the limit one tick inside the spread and flag it **post-only**. The order will either rest and fill as a maker, or be rejected and you re-submit. There is no scenario in which a post-only order quietly becomes a taker order — that is the whole point of the flag.

## A practical fee-minimization checklist

You don't need a complex setup to stop bleeding fees. The bulk of the savings comes from three habits plus one signup decision.

1. **Bind the CASH20 rebate at registration.** The referral code can only be entered during signup — there is no way to add it retroactively. Use the 👉 [CASH20 signup link](https://okx.com/join/CASH20) so the code is pre-filled, complete KYC, and the 20% rebate is active from your very first trade.
2. **Default to post-only limit orders.** One tick inside the spread, post-only flag on, re-submit if rejected. Every fill that lands is a maker fill.
3. **Plan exits in advance.** Prefer stop-limit over stop-market when volatility allows, accepting that occasionally a stop will not fill if price gaps through. Every stop triggered as market is a taker order.
4. **Concentrate volume, don't fragment it.** A round trip chopped into five 20%-size pieces is five separate fee events. If the fragmentation is just nervous over-management, it is a direct fee tax on your account.
5. **Re-check the fee page after any break.** The framework shifts a few times a year — the November 2025 global update and the April 2026 VIP/futures adjustment are recent examples. The cheapest order on Monday is not always the cheapest order in six months.

## Fees that live outside the maker/taker schedule

Three costs don't show up in the maker/taker table and are worth understanding separately:

**Funding** is the periodic payment between longs and shorts on perpetuals, settled every 8 hours on OKX. It is not revenue the exchange keeps, but on a position held for weeks it can outgrow the trading fees of the same trade. No discount program touches it.

**OKX Convert** quotes a rate with no fee line, which means the cost is the spread inside that rate. Past small amounts, a spot limit order is usually the cheaper path.

**Withdrawals** cost a per-coin network fee that OKX adjusts with chain conditions. USDT over TRC20 runs about 1 USDT; the same withdrawal over ERC20 costs a multiple of that when Ethereum is busy. If the wallet or exchange on the other side accepts the cheaper network, the ERC20 premium buys you nothing. Deposits are free, internal transfers between main and sub-accounts are free and instant, and canceling an unfilled order costs nothing — a fee only exists once a trade executes.

## Frequently asked questions

**Are maker fees ever zero or negative?** At very high VIP tiers on certain products, yes — from VIP 6 futures maker is 0.0000%, and from VIP 7 it turns negative, meaning the exchange pays you to provide liquidity. For regular retail users, both maker and taker are positive.

**Does OKX charge fees on cancelled orders?** No. Only fills generate trading fees. Posting and cancelling a limit order — even many in sequence — costs nothing on the fee schedule (there are anti-abuse rate limits, but no per-cancel charge).

**Is the CASH20 rebate temporary?** No. The 20% commission rebate is ongoing for as long as the referral relationship stays active. It is not a limited-time promotion.

**Can I add CASH20 to an existing account?** No. The code can only be applied at registration. Existing accounts cannot retroactively bind a referral code or claim the new-user bonuses tied to it.

**How is my VIP tier decided if I trade multiple products?** You always get the *highest* tier you qualify for across any single product line. A big futures month lifts your spot and options rates too — your best dimension wins, everywhere.

**Are the listed fees the same in every region?** Mostly yes for retail at the regular tier, with regional adjustments and an EEA-specific framework plus a U.S. framework that uses fee groups at the pair level. The fee page automatically shows the schedule for the region you are accessing from — always read it after logging in.

## The short version

The maker/taker spread is the most underestimated line item in a trading account. Get the concepts right, default to post-only limits, bind the rebate at signup, and the fee schedule stops being something that happens *to* you and starts being something you manage. If you're opening an OKX account anyway, 👉 [use the CASH20 code at registration](https://okx.com/join/CASH20) and the math starts working in your favor from the first fill.
