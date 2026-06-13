# Pricing the Impermanent

### Hedging impermanent loss for Uniswap v3 liquidity providers

> Liquidity provision on Uniswap v3 is a short-gamma position. This repository measures the resulting impermanent loss on-chain, prices the offsetting hedge off the listed options surface, and reports the dollar outcome of hedged versus unhedged positions.

**[Full Paper →](Pricing-the-Impermanent_Uniswap-v3-IL-Hedging_Jun26.pdf)**

---

## Summary

Providing liquidity on Uniswap v3 is, in risk terms, a short-convexity trade: the LP is long fee income and short gamma, with arbitrageurs rebalancing the position adversely on every price move. The analysis proceeds in three steps:

1. **Measure** impermanent loss on-chain, position by position.
2. **Price** the offsetting hedge from the Deribit ETH volatility surface (SABR fit with Carr–Madan static replication).
3. **Quantify** the dollar outcome of hedged versus unhedged positions.

The central result: impermanent loss is a priced, tradeable exposure that can be hedged with listed options and financed out of fee income.

---

## 1. The loss is real and measurable

Across **27,529** closed LP positions in five flagship ETH pools, aggregate impermanent loss totals **−$50.7m, with every pool in deficit.** Each position is reconstructed from NonfungiblePositionManager token-IDs and priced at its exact exit-minute. This independently reproduces the Topaz Blue / Bancor finding that roughly half of all v3 LPs underperform simply holding the underlying tokens — a result largely insensitive to range concentration.

## 2. Impermanent loss is short gamma

On every swap the pool sells the appreciating asset and buys the depreciating one — a concave payoff, i.e. short gamma. The corresponding hedge is a long options position. The capped **V3L "Limited"** product resembles downside protection but behaves as a path-dependent barrier rather than a European structure, and its cap is asymmetric: a symmetric 70–130% range locks in a deeper loss on the downside (−10.7%) than the upside (−6.1%).

## 3. Rebalancing crystallises the loss

Rebalancing back into range sustains fee income but converts impermanent loss into realised loss and incurs gas and slippage on each exit. Over 30 days the drag is modest (~10 bp); extended to 90 days or with a tighter band it compounds into the hundreds of basis points. This quantifies the well-known point that once a position is rebalanced, the loss is no longer impermanent.

## 4. Hedged versus unhedged, in dollars

Three representative positions (real pool, median size, measured fee APR):

| Position | Scenario | Unhedged | Hedged |
|---|---|---|---|
| **V2** — full range | +35% move | +$16 | **+$27** |
| **V3R** — concentrated, uncapped | −35% move | **−$419** | −$29 |
| **V3L** — "Limited" cap | −55% move | **−$1,262** | −$111 |

The hedge does not promise gains; it converts a variable, open-ended loss into a fixed, known cost financed by fees. Its value is greatest where the bleed is worst: tight bands, capped products, and sharp moves.

---

## What's next: Impermanent Gain

The companion project ([23d](https://github.com/grantbelford/DEFI-Impermanent-Gain-Modelling)) models **Impermanent Gain (IG)** — the long-gamma mirror an LP buys as a hedge — via the Black–Scholes-tailored LP Greeks of Bardoscia & Nodari (2023). Pairing the LP position with IG produces a net-long volatility book.

## Methodology

- **On-chain data:** Dune Analytics (Trino SQL) — NFPM token-ID reconstruction, single-cycle ≥1-day filter, exit-minute pricing.
- **Pricing:** lognormal (β=1) SABR fit to Deribit ETH options → Carr–Madan static option strip; V3L modelled as a first-passage barrier.
- **Simulation:** 40,000-path SABR Monte Carlo, calibrated to realised volatility, fee APR, and TVL.
- Full methodology and the debugging appendix are in the [paper](Pricing-the-Impermanent_Uniswap-v3-IL-Hedging_Jun26.pdf).

## Citation

Charles G. Belford, June 2026. Builds on Loesch, Hindman, Welch & Richardson (Topaz Blue & Bancor), *Impermanent Loss in Uniswap v3* (2021). On-chain data: Dune Analytics. Query design and exposition assisted by Claude (Opus 4.8).

*This material is for research purposes only and does not constitute financial advice.*
