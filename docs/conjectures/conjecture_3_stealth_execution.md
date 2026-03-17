
Conjecture 3 — Stealth-Optimal Execution Distributions
Execution · Market Impact · Stealth
Motivation
This conjecture has already attracted special interest from a couple of other founders.

The underlying problem is simple to state. Given an algorithmic trading strategy with strong returns, it is almost inevitable that capital will accumulate. As capital scales, order sizes and average position sizes grow — and firms consistently observe that returns deteriorate. A large part of this degradation comes from market impact and loss of execution stealth.

We are therefore looking at two sides of the same coin:

Market impact: how much our orders move prices against us.
Execution stealth: how detectable our trading footprint and intent become to other participants.
Big, obvious orders (for example a single multi-million buy sweep) tend to:

push the order book and short-term imbalance in our disfavoured direction, and
leave a visible footprint that other firms can learn from over time.
This can lead to alpha decay: competitors infer the structure of the strategy, trade ahead of it, or “vulture” around it with sandwich-like behaviour. Execution design is therefore not a side quest — it sits alongside inventory control, risk management, and alpha discovery as a core problem.

This conjecture asks for a principled way to design stealth-optimal execution distributions: how to spread a given order across time, size, and accounts to maximise stealth and minimise slippage / cost.

Informal problem statement
Suppose an algorithmic trading signal (alpha) fires and prescribes a net position change of notional size Q (e.g. Q = 1,000,000 USD) to be executed over an acceptable time horizon T = [0, τ], from signal time t = 0 to a hard time limit τ.

We want to determine:

A joint distribution over execution times and order sizes (and, optionally, accounts) that maximises stealth and minimises slippage / cost over the horizon T.
The order flow here is not the entire portfolio inventory, but the total notional associated with this specific signal. The strategy may trigger repeatedly over time, either to build a position into an attractive opportunity or to gradually unwind into a profit-taking region.

Modelling set-up
We can model an execution schedule in either of two ways:

Discrete trades: a sequence of trades {(tk, qk)} with 0 ≤ tk ≤ τ and ∑ qk = Q.
Continuous order-size density: a function ϕ(t) on [0, τ] such that ∫0τ ϕ(t) dt = Q.
We assume, at minimum, the following microstructure ingredients:

Impact / stealth trade-off (square-root visibility).
The “loss of stealth” or footprint associated with a trade of size q grows approximately as a square-root law, footprint(q) ∝ √|q|. Executing the full notional Q at t = 0 effectively destroys stealth; executing tiny slices (e.g. 10 USD) changes stealth only negligibly.
Account-splitting constraint.
The trader may split Q across at most Nmax = 100 accounts, Q = ∑i=1N Qi with N ≤ Nmax, where each account i follows its own execution schedule. This rules out the degenerate “infinitely many infinitesimal accounts” solution.
Execution cost / slippage.
For simplicity, each individual trade incurs:
a base cost modelled as a fixed k% rate (covering immediate slippage and explicit fees), and
a market impact component consistent with the square-root visibility law above.
Optionally, transaction costs may shrink by a factor c% as order size increases, capturing fee discounts for larger tickets.
Cross-impact and opposite flow.
One may allow for inverse impact or partial cancellation when buy and sell orders of similar size interact in opposite directions over short horizons.
Market “memory” / recycle rate.
The market (and counterparties) can be assumed to have a finite memory horizon: after some time, or after recycling accounts, previous order flow becomes less informative. For example, one could assume:
up to 100 “fresh” accounts can be created per month, and
each new account starts with a “stealth bonus” that decays with its cumulative activity.
Conjecture — Stealth-Optimal Execution Distributions
Under a reasonable microstructure model incorporating:

a square-root–type impact / detectability relationship in trade size,
optional cross-impact between opposing flows,
account-splitting constraints N ≤ 100, and
size-dependent transaction costs,
there exists an optimal joint distribution of:

execution times,
trade sizes, and
account splits (up to 100 accounts),
that:

maximises stealth — in the sense of minimising information leakage or detectability of the underlying strategy; and
minimises expected slippage and total execution cost relative to the alpha’s target price path,
subject to:

t ∈ [0, τ],
∑ qk = Q (or ∫0τ ϕ(t) dt = Q),
N ≤ 100.
Challenge
The challenge is to:

Formulate a precise optimisation problem over time–size–account distributions, specifying:
a stealth / detectability functional,
an impact and cost model, and
any market-memory or recycle-rate dynamics.
Prove existence and (where possible) uniqueness of stealth-optimal execution distributions under this model.
Design explicit algorithms which, given an alpha signal, a total notional Q, and a horizon τ, produce a stealth-optimal execution schedule, including:
how to split Q across up to 100 accounts, and
how to allocate each account’s flow over time.
