


## Products

### stable
Stable asset with a near 100% stable mid price around K.
There will be occasional mispricings, where either bid or ask will move to meet the mid price at K.
If we can assume that this is a simple asset utilised by IMC to get everyone started. The behaviour is simply continuously straightforward. We simply want to market make and market take on given opportunities.

For future we want to do

	1. Market making
	2. Market taking in accordance with fair value
	3. Inventory skew to de-risk


• Uses a fixed fair value of 10,000
• Simply buys below fair value and sells above fair value
• Very straightforward mean-reversion / fair-value taking logic
• Posts both sides around the default price:
	○ buy at 9999
	○ sell at 10001
• This is more of a pure market-making setup than the first team
market-take bids above 10,000 and asks below 10,000,
while also market-making around 10,000 with a chosen edge.

• Uses explicit acceptable bid/ask bounds around 10,000
• Aggressively combines:
	○ market taking when quotes are favourable
	○ market making by undercutting current best price
	○ inventory-dependent quote adjustment
This is much richer execution logic than the first two teams.

Market taking
If an ask appeared below 10,000, it was profitable to buy it.
If a bid appeared above 10,000, it was profitable to sell into it.
These trades were essentially direct arbitrage relative to the known true price. Since the product itself was stable, capturing these mispricings was low risk and mechanically attractive.

Market making
Because obvious mispricings did not happen at every moment, teams also made markets around 10,000 by posting passive bids below fair value and passive asks above fair value. The key question here became how far away from fair value to quote.

That gave rise to the classic market-making tradeoff:

quoting closer to fair increased fill probability,
quoting further away increased profit per fill.
This distance from fair was the edge, and much of the optimization for Rainforest Resin was simply about finding the best edge. Many teams used backtests, visual dashboards, and grid search to determine which quoting distance produced the best overall PnL.

### drift
• Uses a cross / recent-book averaging approach
• Stores old bids and asks
• Computes weighted average bid and ask from recent cached books
• Trades if the current book is favourable relative to these averages by some threshold
• Uses EMA as a dynamic fair value
• Adjusts quotes based on current inventory:
	○ flat position: symmetric around EMA
	○ long position: shifts quotes downward to encourage selling / reduce further buying
	○ short position: shifts quotes upward to encourage buying back
So this team builds inventory-aware quoting directly into the pricing.
• Uses a hard-coded regression forecast on recent cached mid-prices
• Predicts next price from a rolling banana cache
• Converts that forecast into lower/upper acceptable prices
• Then trades and quotes around those bounds
So BANANAS here is the most explicitly forecast-driven of the three.

## Alpha
profit-seeking is always constrained by risk/inventory bounds.
trading edge and breadth.

Even the more advanced code is still built from understandable pieces:
	• fixed fair value
	• moving averages / EMA
	• short rolling regression / cached-price forecast
	• simple undercutting of current best bid/ask
	• inventory skew
So the common style is practical and competition-oriented: simple models with direct execution rules.
• Logic is split into reusable methods like:
	○ reset_from_state
	○ buy_product
	○ sell_product
	○ continuous_buy
continuous_sell

Across the round, the strongest strategies shared the same core elements:

estimate a fair value,
take mispriced orders when the book is favourable,
quote around fair value to earn spread,
and actively manage inventory through soft/hard liquidation or position-clearing rules.



## Memory and features
All of them store some form of historical information:
	• cached bids/asks
	• past prices
	• EMA
	• rolling banana price cache
	• PnL and trade history
So they are not purely reactive to the current book; they are stateful strategies.
• Tracks:
	○ positions
	○ PnL
	○ cash
	○ EMA prices
	○ past prices
• Keeps extensive state:
	○ positions
	○ traded volume
	○ per-product cash PnL
	○ rolling caches
	○ signals from market participants
multiple product-specific parameters


## Fair value estimation
Best bid - best ask
Buy sell orders dict
Available liquidity


Depend on an internal estimate of fair value
Each codebase has some notion of “acceptable” price:
	• fixed fair value for PEARLS
	• moving/estimated fair value for BANANAS via averages, EMA, or regression-like prediction

That is probably the single most important conceptual commonality: they all compare the current market prices to an internally estimated fair value, then trade when the market deviates enough.
a rolling average of the mid price,
or other local smoothing methods.
However, the visible mid price turned out to be noisy because of smaller traders placing orders away from the real center. A better signal was found in the order book itself: there was often a large market-making participant quoting meaningful size on both sides. The midpoint of those large bid/ask quotes provided a much cleaner estimate of fair value and more closely matched the game’s hidden internal valuation. This became a stronger basis for both market taking and market making.

Some teams also explored:

microprice / volume-weighted fair values,
linear regression on STARFRUIT,
and more formal stochastic models such as Ornstein–Uhlenbeck or diffusion-based market making,

This became the central challenge of Kelp.

Some natural first attempts included:

the raw mid-price,
a rolling average of the mid-price,
local smoothing or moving averages,
microprice or volume-weighted fair values.
However, many teams found that the visible mid-price was noisy because smaller participants often placed orders at odd levels that distorted the best bid and best ask. So while the simple midpoint was available, it was not always the cleanest representation of the real consensus price.

This led stronger teams to look deeper into the order book structure. Several better proxies emerged:

the midpoint of large standing bid and ask quotes,
the midpoint of the popular bid and ask prices,
the midpoint of a consistent market maker’s quotes,
filtered order-book levels that ignored small noisy orders.
In other words, instead of trusting the top of the book blindly, teams tried to identify the more meaningful liquidity providers and use their quotes as the fair-value anchor.

Some teams even verified this empirically by comparing their own backtests with the platform’s PnL marking, concluding that the “real” internal fair used by the game was closer to this large-quote midpoint than to the naive visible mid.

current filtered fair value is the best estimate of the next fair value.

## Inventory
Current position
Inventory control. 
Every implementation keeps track of current position and ensures it does not exceed the allowed max long or short position

Combine quoting and taking logic
All three approaches, in one way or another, are about deciding:
	• when to take liquidity because the current price looks favourable
	• when to make liquidity by posting bids and asks around an estimated fair price
So the common trading pattern is not just prediction, but prediction plus execution logic.


Teams used backtesting and grid search to optimize the quoting edge. A key improvement came from adding position-clearing / liquidation logic: when inventory got stuck near the position limit, the algorithm would accept roughly zero-EV trades to reduce exposure and free up capacity for better trades later. This improved PnL by allowing the strategy to keep trading instead of being blocked by position limits.
choosing a better fair value,
optimizing quoting parameters,
and handling inventory intelligently.

Even though the product itself was simple, inventory management still mattered a great deal. A recurring issue was that traders could get stuck at the position limit, meaning they could no longer take profitable opportunities on one side of the market.

To solve that, many teams added some form of position-clearing or liquidation logic. The idea was to sometimes trade at zero edge or near-zero edge — for example, at exactly 10,000 — not because the individual trade was especially profitable, but because it reduced inventory and restored the ability to capture future profitable trades. This often improved performance materially.


## Risk adjustments


## Executions

Once a robust fair-value estimate had been chosen, the strategy for Kelp looked much like Rainforest Resin:

take obvious favourable quotes relative to fair,
place passive quotes around fair,
improve existing quotes by one tick when attractive,
and manage inventory conservatively.
Because Kelp’s spread was typically tighter than Resin’s, the total profit opportunity was smaller. This meant accurate fair-value estimation mattered more, and there was less room for sloppy quoting.
