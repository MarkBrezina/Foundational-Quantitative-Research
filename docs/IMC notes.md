## Products

### Stable product

This is a stable asset with an almost perfectly fixed mid-price around **K**. There are occasional mispricings, where either the bid or the ask moves to a level that effectively meets the true mid-price at **K**.

If we assume this product is intentionally designed by IMC as a simple introductory asset, then its behaviour is fairly straightforward. The objective is to combine market making with market taking whenever obvious opportunities appear.

#### Market making around fair value

The simplest approach is to buy below fair value and sell above fair value.

Typical quotes:

* buy at **9993** - best bid + 1
* sell at **10007** - best ask - 1

This is essentially a pure market-making setup.

#### Market taking around fair value

This is a very straightforward fair-value or mean-reversion strategy:

* buy asks below **10,000**
* sell into bids above **10,000**

If an ask appears below **10,000**, buying it is immediately profitable.
If a bid appears above **10,000**, selling into it is immediately profitable.

These trades are effectively direct arbitrage against a known true price. Because the product itself is stable, capturing these mispricings is low risk and mechanically attractive.

#### Inventory skew and de-risking

Another important layer is inventory-dependent quote adjustment. This creates richer execution logic than simple two-sided quoting.

Because obvious mispricings do not appear at every timestep, teams also post passive bids below fair value and passive asks above fair value. The main question then becomes how far away from fair value to quote.

This creates the classic market-making tradeoff:

* quoting closer to fair value increases fill probability
* quoting further away increases profit per fill

That distance from fair value becomes the **edge**, and much of the optimization for this product is simply about finding the best edge. Many teams used backtests, visual dashboards, and grid search to determine which quoting distance produced the strongest overall PnL.

---

### Drift product

Unlike the stable product, this one requires a dynamic estimate of fair value.

Some of the stronger approaches included:

* using recent order book information and averaging recent bids and asks
* storing past book states
* computing weighted average bid and ask levels from cached books
* trading when the current book looks favourable relative to those averages by some threshold
* using an EMA as a dynamic fair value
* applying hard-coded regression forecasts on recent cached mid-prices

Inventory-aware quoting is also important here:

* **flat position**: quote symmetrically around the EMA
* **long position**: shift quotes downward to encourage selling and reduce further buying
* **short position**: shift quotes upward to encourage buying back

So this type of strategy builds inventory-aware quoting directly into pricing.

Some teams also used short rolling regressions on cached mid-prices to forecast the next price, then converted that forecast into lower and upper acceptable prices. They would then trade and quote around those bounds.

This makes the drift product much more explicitly forecast-driven than the stable one.

---

## Alpha

Profit-seeking is always constrained by risk limits and inventory bounds. Trading is therefore not just about having edge, but also about how broadly and consistently that edge can be deployed.

Even the more advanced codebases are still built from understandable components:

* fixed fair value
* moving averages or EMA
* short rolling regression or cached-price forecasts
* simple undercutting of the current best bid or ask
* inventory skew

So the overall style is practical and competition-oriented: simple models combined with direct execution logic.

Many implementations also split the strategy into reusable methods such as:

* `reset_from_state`
* `buy_product`
* `sell_product`
* `continuous_buy`
* `continuous_sell`

Across the round, the strongest strategies shared the same core ideas:

* estimate fair value
* take mispriced orders when the book is favourable
* quote around fair value to earn spread
* actively manage inventory through soft liquidation, hard liquidation, or position-clearing rules

---

## Memory and features

All of the stronger strategies store some form of historical information, such as:

* cached bids and asks
* past prices
* EMA values
* rolling price caches
* PnL and trade history

So they are not purely reactive to the current order book. They are **stateful strategies**.

Common tracked variables include:

* positions
* PnL
* cash
* EMA prices
* past prices
* traded volume
* per-product cash PnL
* rolling caches
* participant signals
* product-specific parameters

---

## Fair value estimation

The central concept across all codebases is an internal estimate of fair value.

Each strategy has some notion of an “acceptable” price:

* a **fixed fair value** for stable products like PEARLS or Rainforest Resin
* a **moving or estimated fair value** for dynamic products like BANANAS or Kelp, based on averages, EMA, or regression-style forecasts

That is probably the single most important commonality: all strategies compare current market prices to an internally estimated fair value, then trade when the market deviates enough.

Some natural first attempts at estimating fair value included:

* the raw mid-price
* a rolling average of the mid-price
* local smoothing methods
* microprice or volume-weighted fair values

However, many teams found that the visible mid-price was noisy. Small traders often placed orders at odd levels, distorting the best bid and best ask. As a result, the naive midpoint was not always the cleanest representation of the true consensus price.

This led stronger teams to look deeper into the order book. Better proxies for fair value included:

* the midpoint of large standing bid and ask quotes
* the midpoint of the most popular bid and ask prices
* the midpoint of a consistent market maker’s quotes
* filtered order book levels that ignored small noisy orders

In other words, instead of blindly trusting the top of the book, teams tried to identify the more meaningful liquidity providers and use those quotes as the fair-value anchor.

Some teams even tested this empirically by comparing their own backtests with the platform’s PnL marking, and concluded that the game’s hidden internal fair value was often closer to this large-quote midpoint than to the naive visible mid.

A strong working principle became:

**the current filtered fair value is often the best estimate of the next fair value.**

Some teams also explored more advanced approaches, including:

* linear regression on STARFRUIT
* Ornstein–Uhlenbeck style models
* diffusion-based market-making frameworks

For Kelp in particular, fair-value estimation became the central challenge.

---

## Inventory

Inventory control is essential. Every implementation tracks current position and ensures it does not exceed the maximum allowed long or short exposure.

All of the main approaches ultimately combine quoting and taking logic. In one way or another, they are all deciding:

* when to take liquidity because the current price looks favourable
* when to make liquidity by posting bids and asks around an estimated fair price

So the common trading pattern is not just prediction. It is **prediction combined with execution logic**.

A major practical issue is getting stuck at the position limit. Once that happens, the trader can no longer exploit profitable opportunities on one side of the market.

To address this, many teams introduced position-clearing or liquidation logic. The idea is to sometimes accept zero-edge or near-zero-edge trades, not because the trade itself is especially profitable, but because it reduces inventory and frees up capacity for better trades later.

For example, in a stable product with true fair value at **10,000**, trading at **10,000** may have little or no direct edge, but it can still improve overall PnL by reducing inventory and allowing the strategy to keep participating in future profitable trades.

Backtesting and grid search were often used not only to optimize quoting edges, but also to optimize liquidation rules and inventory handling.

Even when the product itself is simple, inventory management matters a great deal.

---

## Execution

Once a robust fair-value estimate has been chosen, the execution logic for a product like Kelp looks similar to Rainforest Resin:

* take obviously favourable quotes relative to fair value
* place passive quotes around fair value
* improve existing quotes by one tick when attractive
* manage inventory conservatively

Because Kelp’s spread is typically tighter than Resin’s, the total profit opportunity is smaller. That means fair-value estimation matters more, and there is less room for poor quoting decisions.

If you want, I can also turn this into a much tighter **repo-style summary** with cleaner headings and less repetition.






---
# Trading system architecture

## Product States

Each product maintains its own state, which acts as the central source of truth for market information and derived features.

### Core responsibilities
- Read raw market data (order book)
- Compute derived features
- Maintain rolling history

### Stored information
- Best bid / ask
- Mid price
- Spread
- Fair value
- Returns
- Volatility
- Trend (multi-horizon)
- Mean reversion distance
- Order imbalance
- Rolling price / return / volume history
- and more


### Role in system

ProductState is **purely informational**:
- No trading decisions
- No risk decisions
- Only transforms raw data → structured signals

## Strategies

Strategies operate on ProductState and produce **QuoteIntent** (or later, target inventory).

Each strategy represents a distinct source of alpha.

### General principle

Strategies:

- Do not consider hard limits
- Do not enforce risk constraints
- Express desired behaviour, not final execution

## 1. Market Making

A simple spread-capturing strategy.

**Logic** 
- Read current best bid / best ask
- Post:
   - bid slightly above best bid
   - ask slightly below best ask
- If spread is too tight, revert to quoting around fair value

**Objective**

Earn the spread consistently through passive liquidity provision.

## 2. Value Arbitrage

A fair-value crossing strategy.

**Logic**
- If **bid > fair value** → sell into it
- If **ask < fair value** → buy into it
- Position is expected to unwind as price returns to fair value
**Behaviour**
- Aggressive (taker)
- Short holding period
- Relies on immediate correction or microstructure inefficiency

## 3. Mean Reversion

A distance-from-equilibrium strategy.

### Logic
- Define equilibrium (fair value or smoothed price)
- Compute distance:
   - Negative distance → price too low → buy
   - Positive distance → price too high → sell

### Behaviour
- Builds inventory gradually when far from equilibrium
- Reduces or reverses near equilibrium
- Works best in oscillatory or range-bound regimes

## 4. Trend Strategy

A directional inventory strategy.

### Logic
Compute trend (multi-window or weighted)
- If trend is positive → accumulate long inventory
- If trend is negative → accumulate short inventory
- If trend weakens toward zero → reduce exposure
### Behaviour
- Persistent directional bias
- Inventory-driven rather than quote-driven
- Should unwind when signal weakens
### Strategy Outputs

All strategies return QuoteIntent:
- desired bid / ask
- desired size
- source / weight

Later evolution:
- Trend + Mean Reversion may instead return target inventory
- 
## Inventory System

The inventory system manages **position constraints and skewing.**

### Responsibilities
- Track current position
- Enforce:
  - hard limits (absolute cap)
   - soft limits (early warning)
- Compute inventory skew
- Adjust quotes based on exposure

### Behaviour
- As inventory increases:
   - reduce same-side quoting
   - encourage opposite-side trades
- Near limits:
   - suppress risk-increasing orders

### Role in system
Inventory is a **shaping layer**, not an alpha source:
- modifies strategy output
- does not generate independent signals

## Risk System

The risk system evaluates whether current and proposed positions are acceptable.

### Inputs
- Current position
- Volatility
- Trend
- Mean reversion signal
- Simulated price paths

### Metrics
- Probability of loss
- VaR / CVaR
- Expected PnL
- Adverse move probability

### Logic
- Simulate forward price paths
- Evaluate inventory under those scenarios
- Determine whether:
   - risk is acceptable
   - exposure should be reduced
   - new inventory can be added

### Outputs
- RiskReport containing:
   - limits
   - probabilities
   - recommended action

### Behaviour
- If risk is high:
   - suppress new orders
   - bias toward flattening
- If risk is acceptable:
   - allow strategy + inventory adjusted quotes

### Role in system

Risk acts as a **final gatekeeper**:
- can override all strategies
- can force flattening

## Strategy Aggregation

Multiple strategies may generate quotes simultaneously.

### Combination logic
- Bid = most aggressive (highest)
- Ask = most aggressive (lowest)
- Sizes = aggregated or weighted

### Purpose
- Merge multiple alpha sources into a single executable intent

## Trader Setup
### On initialization

For each product:

1. Load parameters from *PARAMS*
   - hard limits
   - soft limits
   - initial fair value
   - strategy parameters
2. Create *ProductState*
3. Initialize systems:
   - *InventorySystem*
   - *RiskSystem*
5. Assign strategies per product:
```
self.strategies = {
    Product.A: [MarketMaking, MeanReversionStrategy(), ValueArbitrageStrategy()],
    Product.B: [MarketMaking, TrendStrategy(), MeanReversionStrategy(), ValueArbitrageStrategy()],
}
```

## Trader.run(state)

This is the central execution loop.

## Per product:
### 1. Load market state
- Read order_depth
- Update ProductState
### 2. Update features
- Compute trend, volatility, MR signals, etc.
### 3. Evaluate risk
- Generate RiskReport
### 4. Severe risk check
If risk is severe:
- skip strategy execution
- generate flattening / reducing quotes only

### 5. Generate strategy quotes
- Each strategy produces a QuoteIntent
### 6. Combine strategy outputs
- Aggregate into a single intent
### 7. Apply inventory shaping
- skew quotes
- adjust sizes
### 8. Apply risk approval
- suppress unsafe orders
- enforce limits
### 9. Execute
- convert approved quotes into orders
- send to exchange
### 10. Update memory
- persist product states
- persist inventory / PnL tracking



## System Flow Summary
```
Market Data
   ↓
ProductState (features)
   ↓
Strategies (alpha signals)
   ↓
Strategy Aggregation
   ↓
Inventory System (position shaping)
   ↓
Risk System (approval / suppression)
   ↓
Order Builder
   ↓
Execution
```

## Key Design Principles
### 1. Separation of concerns
- ProductState → data
- Strategies → alpha
- Inventory → constraints
- Risk → safety

### 2. Strategies do not manage risk
They express intent only.

### 3. Risk can override everything
Final authority on whether orders are allowed.

### 4. Inventory shapes, not decides
It modifies behaviour but does not generate alpha.

### 5. Multi-layer architecture
- micro (order book)
- meso (strategy)
- macro (risk + inventory)


## Future Improvements
- Convert Trend / Mean Reversion to **target inventory framework**
- Add regime detection (trend vs mean-reverting state)
- Improve simulation (non-GBM, order flow driven)
- Add execution layer:
   - maker vs taker logic
   - stealth execution
   - queue priority optimisation
