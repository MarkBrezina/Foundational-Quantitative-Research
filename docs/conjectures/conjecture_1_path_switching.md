Conjecture 1 — Dynamic reallocation between strategies under path switching
Stochastic Processes · Path Stitching · Strategy Reallocation
Consider a single risky asset with price process (St) observed at discrete times 0 = t0 < t1 < … < tN = T. We are given a family of models {ℳk} under which we can simulate Monte Carlo paths, and a finite collection of trading strategies {π¹, …, πK} (e.g. mean-reversion, trend-following).

Initial regime (0 → T₁). At time t = 0, choose an initial model ℳ₁ and simulate a set of paths {X(1,i)t} on [0, T₁]. Using a chosen objective (e.g. risk-adjusted return), select a “best” strategy π(1) (for example, mean-reversion) and trade according to its position trajectory θ(1)t over [0, T₁]. Once realised prices on [0, T₁] are known, we can evaluate how well (ℳ₁, π(1)) fit the realised path.
Event and model update at T₂. At some later time T₂ ∈ (T₁, T), a significant event or regime change occurs. The original model ℳ₁ and strategy π(1) are no longer believed to be optimal beyond T₂. We adopt a new model ℳ₂ and a new “target” strategy π(2) (for example, trend-following) with position trajectory θ(2)t on [T₂, T₃], where T₃ > T₂ is the time by which the transition to the new regime should be complete.
Re-simulation in the new regime (T₂ → T₃). Starting from the realised price ST₂, simulate a new family of paths {X(2,j)t} on [T₂, T₃] under ℳ₂ and obtain the positions implied by π(2). The portfolio at T₂ currently holds the legacy position θ(1)T₂, but we want to move toward θ(2)t over [T₂, T₃], subject to frictions (transaction costs, liquidity, risk limits).
The core problem on [T₂, T₃] is to define a dynamically consistent transition from the old position to the new one. For instance, introducing a control process wt ∈ [0, 1] representing the fraction of capital allocated to the new regime, the realised position at time t could be written as

θt = (1 − wt) · θ(1)t + wt · θ(2)t,   t ∈ [T₂, T₃].

We then seek a rule for wt (or, in discrete time, for allocations at t₁, t₂, …, tn between T₂ and T₃) that trades off:

closeness to the new “optimal” path θ(2)t,
trading costs and turnover, and
risk and capital constraints along the entire transition.
Challenge. Given a sequence of model/strategy pairs (ℳ₁, π(1)), (ℳ₂, π(2)), … and simulated paths plus realised prices at update times 0 < T₁ < T₂ < T₃ < …, design either:

A mathematical framework (loss functional, cost functional, and a notion of “shortest” or most coherent adjustment between position paths), or
An explicit algorithm that, at each update time Tk, outputs a dynamically consistent sequence of positions θt₁, …, θtn or allocations between strategies on [Tk, Tk+1],
describing how capital should be reallocated from the old regime to the new one in a way that is path-wise coherent, cost-efficient, and robust across Monte Carlo scenarios.
