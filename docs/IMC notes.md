MarkBrezina
markbrezina
Vil ikke forstyrres

MarkBrezina — i går kl. 22.17

MarkBrezina — i går kl. 22.52
Welp, nope
I'll try market making with one of them and delta-hedged market making on the other for now
R — i går kl. 23.02
Shame that would have been pretty interesting
MarkBrezina — i går kl. 23.04
yeah
I'm working on the delta-hedged market making
R — i går kl. 23.05
Do you need to upload anything? I'm going to run a few files through to test some things
MarkBrezina — i går kl. 23.10
I'll work for a bit and than submit something, before getting a bit of sleep
R — i går kl. 23.12
Ok
R — i går kl. 23.38
Looking these the previous year repos is hilarious
The alpha animals team screwed up into doing well
“Due to an unexpected bug in our code, we ended up shorting volcanic rock at the max position limit for the entire duration of the trading day. Fortunately for us, this strategy ended up working, bringing our ranking to 2nd in the world!”
The US winners (7th place) also botched the round 
R — 00.41
Wow this round is short 
2 days instead of the usual 3
MarkBrezina — 08.09
Yeah, we have 2 days instead
R — 08.09
Iv scalping was a fail for me
Also yea pairs trading didnt work
MarkBrezina — 08.10
Yeah neither here
R — 08.10
I should have the calibration soon though
MarkBrezina — 08.10
Some very simple delta hedging got me to 1.3K
R — 08.10
They destroyed the liquidity this year
MarkBrezina — 08.10
What do you mean?
No fills?
R — 08.11
Videresendt
Last prosperities all had very liquid exchsnges, so you might have noticed we have made some a bit more illiquid for a change!

IMC Prosperity  •  04.18
MarkBrezina — 08.11
Interesting
R — 08.12
Videresendt
anywhere from 10k to 25k if i had to guess

IMC Prosperity  •  05.30
MarkBrezina — 08.26
Alright. So the spreads might be tight as well
R — 08.41
Yea
I should have some stuff for you soon (hopefully)
MarkBrezina — 09.19
Got ChatGPT to run it all over in the night
Filtype for vedhæng: acrobat
Trading Data Analysis for Velvetfruit Extract and Hydrogel Pack.pdf
179.23 KB
R — 09.20
Gpt 5.5 model? 
MarkBrezina — 09.21
I believe so
R — 09.22
In a little bit ill send over a calibration report and the mc backtester along with a much shorter report myself
MarkBrezina — 09.23
nice
R — 09.36
# Round 3 Calibration — Unified Consensus (Claude + GPT + Teammate Analysis)

Three independent analyses were run on this round's data. This document is the merged consensus with disagreements flagged.

## Who Analyzed What
- **Claude**: Sandbox hold-1 FV extraction, historical IV surface analysis, bot structure, R2 postmortem-informed strategy design

round3_unified_calibration.md
9 KB
﻿
# Round 3 Calibration — Unified Consensus (Claude + GPT + Teammate Analysis)

Three independent analyses were run on this round's data. This document is the merged consensus with disagreements flagged.

## Who Analyzed What
- **Claude**: Sandbox hold-1 FV extraction, historical IV surface analysis, bot structure, R2 postmortem-informed strategy design
- **GPT (ChatGPT o3)**: Same sandbox data + historical notebook analysis, backtest trader hypotheses, parameter stability advice
- **Teammate (PDF)**: Deep microstructure analysis of the raw data file, cross-asset relationships, book churn analysis, strategy prioritization

## Calibration: Full Agreement

All three analyses agree on:

### VELVETFRUIT_EXTRACT (VE)
- Random walk, σ ≈ 0.97/tick, no drift
- Spread: mean 5, median 5
- Always two-sided book
- Server FV tracks observable mid extremely closely (mean diff 0.01, std 0.46)
- Buy/sell hold-1 symmetry confirmed
- No position-dependent FV from hold-max experiment
- **Role**: Underlying for all vouchers + hedge instrument + standalone MM candidate

### HYDROGEL_PACK (HP)
- Random walk, σ ≈ 1.93/tick, **NOT fixed at 10000**
- Spread: mean 15.7, median 16
- Always two-sided book
- **Role**: Standalone wide-spread market making (NOT a pair trade with VE — return correlation 0.006)

### VOUCHERS
All 10 vouchers have dynamic server FV tracking Black-Scholes. Buy/sell symmetry confirmed for all tested pairs.

#### IV Surface (trained on days 0+1 only, day 2 held out per iawa's advice)
```
IV(m_t) = 0.0269 × m_t² - 0.000269 × m_t + 0.2394
m_t = log(S/K) / √(TTE_years)
```
The smile is nearly flat. A flat-IV model (rolling average of mid-IVs across strikes) works nearly as well.

#### The VEV_5400 Signal
All three analyses independently identified VEV_5400 as the strongest single-strike alpha:

| Source | Finding |
|--------|---------|
| Claude | -1.13% mean IV residual, 2.8σ persistent, 99% of ticks negative |
| GPT | "cheap outlier", -1.13% mean, std 0.004, "best-looking alpha" |
| Teammate PDF | Did not compute IV residuals per-strike but identified VEV_5200-5300 as the "key ATM/near-ATM relative-value instruments" |

Day stability: Day 0 mean = -0.01123, Day 1 mean = -0.01130. **Nearly identical.** The signal does not degrade across days — it gets MORE consistent (std drops from 0.0054 to 0.0023).

#### Realized Vol vs Implied Vol
| Day | RV (annualized) | Avg ATM IV | Ratio |
|-----|-----------------|------------|-------|
| 0 | 0.408 | 0.233 | 1.75× |
| 1 | 0.413 | 0.234 | 1.77× |

Options are systematically cheap vs realized moves. All three analyses flag this but note it may be intentional simulation design.

#### TTE Formula (Critical — get this wrong and every option price is off)
```python
# For the SCORED Round 3 simulation (TTE_START = 5 days):
TTE_days = 5.0 - timestamp / 1_000_000
T = TTE_days / 365.0

# For historical data:
# Day 0: TTE_START = 8, Day 1: TTE_START = 7, Day 2: TTE_START = 6
```

## GPT Backtest Results (days 0+1 only)

| Trader | Day 0 PnL | Day 1 PnL | HP PnL | Options PnL | Notes |
|--------|-----------|-----------|--------|-------------|-------|
| HP only MM | 6,196 | 11,855 | 18,051 | 0 | Pure HP, no options |
| 5400 long bias + HP | 6,466 | 11,557 | 19,137 | -1,114 | HP carries it, 5400 LOSES |
| 5300/5400 vertical | -773 | -205 | 0 | -978 | Pure loss |
| Full stack (options + HP) | 5,135 | 10,922 | 17,264 | -1,208 | HP carries it, options net negative |

### The Brutal Truth From These Backtests

**HP is doing all the work.** In every trader, HP accounts for 95-100% of PnL. The options strategies are net negative or marginal.

**VEV_5400 long bias lost money.** -949 on day 0, -165 on day 1. Despite being "structurally cheap" by the IV residual analysis, crossing the spread to buy it doesn't recover because the spread is 1-2 ticks and the IV edge is only ~2.2 price points per unit. With 24 trades on day 0 averaging ~40 units, the spread cost (~24 × 40 × 1 = 960) roughly equals the loss.

**The vertical (short 5300 / long 5400) lost on both days.** The leg risk and spread costs dominate the ~1.5 vol point relative-value signal.

**Full stack options are marginal.** VEV_5100 made 2164 on day 1 but lost 1031 on VE hedging and 986 on VEV_5300. Net options contribution: near zero or negative.

## Disagreements

### Claude vs GPT

| Topic | Claude | GPT |
|-------|--------|-----|
| VEV_5400 as Tier 1 | Rolling mid-IV MM across ALL strikes should be Tier 1; 5400 bias within that | 5400 long bias is Tier 1, rolling MM is Tier 3 |
| HP priority | Tier 2 (free proven PnL) | Tier 4 |
| Vertical (5300/5400) | Against it (leg risk, 2× position, asymmetric signal quality) | Tier 2 |
| Server IV gap | Initially thought server IV ~0.27 > market 0.23 (corrected: measurement error) | Correctly skeptical from the start |

**GPT's backtest results vindicate Claude's skepticism of the vertical and support Claude's HP prioritization.** The vertical lost money on both days. HP earned 6-12k per day consistently.

### Both vs Teammate PDF

The teammate's PDF analysis is the most rigorous microstructure work but reaches similar strategic conclusions. Key additions from the PDF:
- VE-HP return correlation is 0.006 — no pair trade
- Book churn is extremely high (90-99.7% of snapshots change) — passive queue position decays fast
- Trade-print fields in the CSV are duplicated across all products (market-wide summary, not per-instrument) — **cannot use last_price for per-product analysis**
- No monotonicity or convexity violations in the call ladder — no static arbitrage available
- VEV_6000 and VEV_6500 have NO bid in 100% of snapshots — they're one-sided ask books

## Revised Strategy Stack (Post-Backtest)

Given that HP is empirically the biggest PnL source and options are marginal:

### Tier 1: HP Wide-Spread Market Making (~6-12k per day)
- Quote around rolling mid with inventory skew
- Spread ~16, σ ≈ 1.93/tick — this is the proven R2 framework
- This is 95% of GPT's backtest PnL across all traders

### Tier 2: Rolling Flat-IV Options MM on VEV_5000-5500 (~0-3k per day)
- Compute rolling mid-IV (exclude VEV_5400 from the anchor since it's the outlier)
- Fair price = BS(VE_mid, K, T, rolling_IV) + bias for 5400
- Buy below fair - threshold, sell above fair + threshold
- Keep thresholds conservative (edge ≥ 1.5-2.5 depending on strike)
- **DO NOT cross the spread for VEV_5400 just because it's cheap — the backtest shows this loses money**
- Instead: place passive bids at fair-0.5 to fair+0.5 for 5400, let the market come to you

### Tier 3: VE Market Making (~0-1k per day)
- Tight spread (5), σ ≈ 0.97, similar to R2 ASH
- Also serves as hedge instrument for options delta

### Tier 4: Delta Hedging (cost center, not profit center)
- Only when net option delta exceeds ±75 VE equivalent
- Hedge in chunks of 20, not continuously
- Each hedge costs ~2.5 spread — don't over-hedge

### Tier 5: VEV_5400 Structural Long Bias (within Tier 2 MM)
- Do NOT make this a standalone strategy
- Within the rolling-IV MM, give VEV_5400 a +0.75 fair price bias
- Lower the buy threshold by 0.3 vs other strikes
- But never cross the ask unless edge ≥ 2.0

## iawa's PnL Benchmarks
- **300k across 3 days of backtesting**: "absolute minimum good"
- **10k-40k on the website** (1k ticks): realistic range
- **Oracle cap**: 155k on 1k ticks (anyone higher is overfit)
- **Day 2 = holdout validation**: don't touch until params are frozen

The HP MM alone gives 18k/day on backtest = 54k across 3 days. We need the options MM to add 80k+/day to hit 300k. The current options implementation isn't there yet. The gap is in execution quality, not signal quality — the 5400 signal is real, but the trading mechanics need to capture it without bleeding spread costs.

## Open Questions
1. Can passive-only quoting on VEV_5400 capture the structural cheapness without crossing the spread?
2. Does the RV/IV gap (1.75×) translate to tradeable vol premium after hedge costs?
3. What happens on the server's scored run vs the backtester? (R2 taught us these can differ significantly)
4. The "sandbox alpha" iawa mentioned — we still haven't identified what it is beyond FV extraction
round3_unified_calibration.md
9 KB
