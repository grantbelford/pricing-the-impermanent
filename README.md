# Pricing the Impermanent

**An Option-Implied Framework for Hedging Uniswap v3 Liquidity-Provider Loss**

Charles G. Belford · On-chain data: Dune Analytics · June 2026

📄 **[Read the paper →](Pricing-the-Impermanent_Uniswap-v3-IL-Hedging_Jun26.pdf)**

---

## Summary

Topaz Blue & Bancor (2021) showed that, in aggregate, Uniswap v3 liquidity providers (LPs)
lost more to impermanent loss (IL) than they earned in fees — an empirical, backward-looking
diagnosis. This work extends it three ways:

1. **An independent, position-level re-measurement** across five major ETH pools confirms the
   result — **$50.7m of IL across 27,529 closed positions, every pool negative** — using a
   cleaner estimator built on NonfungiblePositionManager token-IDs, valued at each position's
   exact exit-minute price.
2. **IL is formalised as a short-gamma options position** and the protection is priced off the
   live Deribit volatility surface via SABR-calibrated static (Carr–Madan) replication, treating
   the capped "Limited" (V3L) product as a **first-passage barrier**, not a European condor.
3. **The rebalancing "gamma bleed"** the original authors flagged as future work is quantified
   with a Monte-Carlo calibrated to realised volatility, fee APR, and TVL computed live on
   **Dune Analytics**.

The result is a forward-looking, market-implied framework for pricing and hedging LP loss, with
a concrete fee-cover/breakeven table for the product economics.

## Key result

Across **27,529** real closed single-cycle LP positions (held ≥ 1 day) in five flagship ETH pools,
measured impermanent loss is **negative in every pool**, totalling **−$50.7m** — a direct,
position-level confirmation that the unhedged LP loses, and the empirical case for a tradeable IL hedge.

## Acknowledgement

Query design, debugging, and exposition were carried out with the assistance of Claude AI (Opus 4.8).
On-chain data from Dune Analytics. Builds on Topaz Blue & Bancor, *Impermanent Loss in Uniswap v3* (2021).
