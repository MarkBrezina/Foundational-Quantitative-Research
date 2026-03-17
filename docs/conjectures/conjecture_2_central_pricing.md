Conjecture 2 — The Central Pricing Problem
Pricing · Market Microstructure · Mean-Field Effects
We extend the Central Price Problem from a single asset to a system of N > 3 interacting assets.

Setup
Consider N > 3 generalised assets with stochastic price paths {P(i)t}, i = 1, …, N, observed at discrete times 0 = t0 < t1 < … < tT, with prices known for all t ≤ T and unknown for t > T.

For concreteness, one may think of an underlying asset A, its option A* and swap A ̃; another underlying B with B* and B ̃; and more generally underlying assets A, B, C, D, … with associated derivatives A*, A ̃, B*, B ̃ and so on.

Immediate vs historical dependence. The one-step-ahead price of each asset admits a decomposition P(i)T+1 as a weighted combination of an “immediate impact” term and a “historical dynamics” term. The relative weight α(i)T ∈ [0, 1] may vary over time, but the weights always sum to one.
Historical dynamics. The historical term may depend on past prices {P(j)t} for t ≤ T, cross-asset and cross-derivative correlations, order flow, liquidity, volatility regimes, fundamentals, and longer-horizon features such as drift and mean reversion.
Immediate impact and behavioural effects. The immediate impact term captures order-book mechanics and behavioural/flow effects, such as large buy orders pushing prices upward, short-horizon supply–demand imbalances, and stylised mean-reversion patterns around local extremes, while still allowing for a persistent long-horizon drift.
Information set. The modeller has access to (i) the full joint price and volume history of all N assets up to time T, (ii) the financial statements and fundamentals of the corresponding issuers, and (iii) the aggregate positions of other market participants, so that pricing must be treated as a mean-field problem rather than a single-agent one.
Own-price impact. Your orders move the mid-price according to a simple impact rule, for example: new_mid = current_mid + (your_price - current_mid) * sqrt(order_volume) . This is a toy model; researchers are free to propose more realistic impact specifications.
Statement of the Conjecture
Under the multi-asset, mean-field setting above, the conjecture states that there exists:

Short-horizon central price estimator. A statistically significant and practically accurate formula (or class of models) for the “central” or “immediate” price of each asset at time T + 1, constructed from observable market and participant features at or before time T.
Multi-step confidence regions. A statistical procedure that produces confidence intervals (or more general confidence sets) for the joint price paths {P(i)t} over t = T + 1, …, T + H, with H ≥ 5 and i = 1, …, N, achieving empirically valid coverage probabilities (“reasonable accuracy”) under realistic market conditions.
In other words, there should exist a non-trivial, empirically testable mapping from historical dynamics (prices, volumes, correlations, regimes, fundamentals) and immediate impact (order-book state, flows, participant positions, your own trades) to short-term central prices and medium- horizon price ranges for a system of interacting assets.

Research Note
Special interest is reserved for solutions that embed this multi-asset formulation into a unified pricing framework that simultaneously covers:

the market-maker’s very short-horizon pricing problem,
the high-frequency trading pricing problem, and
the medium- to long-term asset pricing problem,
under a common set of assumptions linking microstructure, participant behaviour and longer-term price formation.
