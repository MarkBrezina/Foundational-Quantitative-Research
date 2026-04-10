


## Products

### Emeralds - stable
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


• Uses explicit acceptable bid/ask bounds around 10,000
• Aggressively combines:
	○ market taking when quotes are favourable
	○ market making by undercutting current best price
	○ inventory-dependent quote adjustment
This is much richer execution logic than the first two teams.



### Tomatoes - drift
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
continuous_sell<img width="933" height="504" alt="image" src="https://github.com/user-attachments/assets/9acaa5f8-5b95-4ea0-b416-dbf610b8a0d7" />



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


## Inventory
Current position
Inventory control. 
Every implementation keeps track of current position and ensures it does not exceed the allowed max long or short position

Combine quoting and taking logic
All three approaches, in one way or another, are about deciding:
	• when to take liquidity because the current price looks favourable
	• when to make liquidity by posting bids and asks around an estimated fair price
So the common trading pattern is not just prediction, but prediction plus execution logic.<img width="912" height="303" alt="image" src="https://github.com/user-attachments/assets/bce79218-f134-448d-a197-ae4240327564" />



## Risk adjustments
