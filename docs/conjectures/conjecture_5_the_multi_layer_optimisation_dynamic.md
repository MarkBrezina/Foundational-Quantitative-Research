Conjecture 5 — The Final Problem
Portfolio · Optimisation · Multiscale
Modern financial firms are built as hierarchies of decision makers: firm → portfolio → strategy → execution. Each layer has its own objectives, constraints and key performance indicators, yet all of them act on the same underlying market and the same pool of capital.

Fix a single firm-wide portfolio A, with total capital V, trading over a universe of assets with associated features (prices, returns, volatilities, liquidity, funding, fundamentals and microstructure attributes). The portfolio A is decomposed into internal components:

strategy pods and sub-portfolios,
risk and hedging books,
execution and routing policies,
and any further sub-components (desks, regions, time-horizons).
Each component X in this hierarchy:

controls a local state (positions, risk exposures, cash, inventory),
faces its own constraints (risk limits, turnover, liquidity, leverage, drawdown, stealth, capital budget),
and optimises a local objective functional (e.g. Sharpe, PnL, risk-adjusted return, tracking error, execution cost).
At the same time, the firm as a whole optimises a global objective functional over the full configuration of all components: long-horizon risk–return, drawdown and survival probabilities, capital efficiency, and systemic risk.

Problem.
Specify a mathematical framework (objective, constraints and decision rules) that, given:

the hierarchical decomposition of the firm (components and sub-components),
the feature set of all traded assets and strategies, and
the evolving market state and information flow,
produces a configuration of decisions (capital allocations, positions, risk limits and execution policies) that is simultaneously:

globally optimal for the firm-wide objective (risk–return, survival, capital efficiency), and
locally optimal for every component and sub-component, given its constraints and its local view of the environment.
In other words, the same capital and information must support a configuration where the whole portfolio A is optimal inside the market, and each internal piece of A is optimal inside its own local neighbourhood of constraints and interactions.

The conjecture.
There exists a principled, implementable multiscale optimisation principle — a single rule, or variational framework — that maps the full state of the market and the firm (prices, returns, risks, liquidity, fundamentals, correlations and hierarchical structure) into such a configuration of decisions, and that:

applies uniformly across layers (firm, portfolio, strategy, execution),
respects all local constraints and risk budgets, and
yields configurations that are stable under feedback from market dynamics and from the actions of other agents.
The challenge is to write down this multiscale principle explicitly and to show that, under realistic assumptions on markets and constraints, its solutions are indeed simultaneously globally and locally optimal in the above sense.
