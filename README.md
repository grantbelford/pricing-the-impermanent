# Pricing the Impermanent 🩸

### *or: how to stop getting **rekt** as a Uniswap v3 liquidity provider*

> **gm ser.** You aped into an LP position for that juicy fee APR. The arbitragoors said *thank you*.
> This repo measures the bleeding, prices the antidote off the options smile, and shows — in dollars —
> what hedging your impermanent loss would actually have done.

📄 **[Read the full paper — 6 pages, neon-on-black, all charts inside →](Pricing-the-Impermanent_Uniswap-v3-IL-Hedging_Jun26.pdf)**

---

## TL;DR

Providing liquidity on Uniswap v3 is a **short-gamma** trade wearing a yield-farm costume. You're short
convexity, long fees, and every time price moves an **arbitrageur rebalances your bag against you**. We:

1. **Re-measure the carnage** on-chain — position by position, no hand-waving.
2. **Price the antidote** off the Deribit vol surface — static snapshot (SABR + Carr–Madan static replication).
3. **Show the dollars** — hedged vs unhedged, on real-shaped positions.

Spoiler: the loss is a *priced, tradeable* exposure. You can buy protection and **pay for it out of fees**.

---

## 1. The rekt is real 💀

Across **27,529** real closed LP positions in 5 flagship ETH pools: **−$50.7m of impermanent loss, every
single pool in the red.** Not vibes — measured position-by-position from NonfungiblePositionManager
token-IDs, each one priced at its *exact exit-minute*. This independently reproduces the OG **Topaz Blue /
Bancor** finding that ~half of all v3 LPs underperform just diamond-handing the tokens. The silent killer
is real, and it doesn't care how concentrated your range is.

## 2. IL is just short gamma (the arbitragoor's free lunch) 🥪

The pool *sells you the winner and buys you the loser* on every swap — a concave payoff, i.e. **short
gamma**. The protection an LP wants is the mirror image: a long options position. The capped **V3L
"Limited"** product looks like it caps your downside (and it does), but it's a **path-dependent barrier**,
not a tidy European condor — and the cap is **asymmetric**: a symmetric 70–130% range locks a *deeper*
loss on the way down (−10.7%) than up (−6.1%). Ngmi if you assumed symmetry.

## 3. The gamma bleed (death by a thousand rebalances) 🩸

Rebalancing back into range keeps fees flowing — but it **crystallises** the IL and pays gas/slippage every
exit. At 30 days the drag is tiny (~10 bp); push the horizon to 90 days or tighten the band and it
**compounds to hundreds of bp**. The "once you rebalance, the loss is no longer impermanent" point the
original authors flagged — now quantified.

## 4. Hedged vs getting rekt — in actual dollars 💸

Three representative degens (real pool, median size, measured fee APR):

| | position | what happened | unhedged | hedged |
|---|---|---|---|---|
| **V2** | full-range fren | +35% pump | +$16 | **+$27** |
| **V3R** | concentrated chad | −35% dump (uncapped — keeps bleeding) | **−$419** | −$29 |
| **V3L** | "Limited" cap | −55% nuke (IL frozen at the cap) | **−$1,262** | −$111 |

The hedge never promises number-go-up; it **converts a variable, open-ended IL into a fixed, pre-known
cost** — financed by fees. It earns its keep exactly where the bleed is worst: tight bands, capped
products, sharp moves.

---

## What's next → Impermanent **Gain** 📈

The sequel ([23d](https://github.com/grantbelford/DEFI-Impermanent-Gain-Modelling)) flips the trade: modelling
**Impermanent Gain (IG)**, the long-gamma mirror an LP *buys* to hedge (and a vol trader buys to bet on
chaos), via the Black–Scholes-tailored LP Greeks of Bardoscia & Nodari (2023). Hedge the LP with IG and
the book turns **net-long volatility**. wagmi.

## Under the hood 🔧

- **On-chain:** Dune Analytics (Trino SQL) — NFPM token-ID reconstruction, single-cycle ≥1-day filter, exit-minute pricing.
- **Pricing:** lognormal (β=1) SABR fit to Deribit ETH options → Carr–Madan static option strip; V3L as a first-passage barrier.
- **Sim:** 40k-path SABR Monte-Carlo, calibrated to realised vol + fee APR + TVL.
- Methodology + the 8-step debugging saga are in [the paper's appendix](Pricing-the-Impermanent_Uniswap-v3-IL-Hedging_Jun26.pdf).

## Cite / acknowledge

Charles G. Belford · June 2026. Builds on Loesch, Hindman, Welch & Richardson (Topaz Blue & Bancor),
*Impermanent Loss in Uniswap v3* (2021). On-chain data: Dune Analytics. Query design & exposition
assisted by Claude (Opus 4.8).

*Not financial advice. DYOR. If you LP unhedged and get rekt, that's on you, ser.* 🫡
