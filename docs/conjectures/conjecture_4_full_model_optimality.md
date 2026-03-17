
Conjecture 4 — Existence of Optimal Strategies in the Full Multi-Layer Market Model
Hybrid geometry · Mean-field games · Optimal control
We model financial markets as a coupled, multi-layer system: a time-varying market/asset graph and value surface, a network of interacting agents with utilities and information links, a hierarchy of rules governing dynamics, and (in the most general version) a geometric / hybrid frame where agents move on a manifold with continuous flows and discrete jumps.

In a simplified, finite, fully discrete version of this environment, the decision problem of a fully-informed agent reduces to a Markov Decision Process, and classical Bellman theory guarantees existence of an optimal policy, computable by dynamic programming. The conjecture asks whether an analogous guarantee holds in the full geometric–informational, hybrid, multi-agent setting, where the state is infinite-dimensional (fields + agents), time is hybrid (flows + jumps), and interactions are game-theoretic / mean-field rather than a single MDP.

Informal statement
Can we prove that, in the full Tomorrow Markets framework, there exists at least one optimal strategy (or equilibrium strategy profile) for agents, and characterize it in a reasonably “simple” class (Markov, feedback, or geodesic / variational policies)?

In other words: once you accept the market model as a hybrid geometric–informational multi-agent system, is it mathematically guaranteed that there is a best possible way to trade (or a best equilibrium configuration of strategies) consistent with the rules of the world?

Formal version (single optimal agent)
Let Xt denote the global state (fields, graphs, agents) evolving as a hybrid dynamical system with flows and jumps on X = M × S, where M is a (possibly time-varying) manifold and S indexes discrete modes / frames. A distinguished “Tomorrow” agent observes Xt and chooses controls at ∈ A(Xt).

The agent’s monetary performance under policy π is

J(π) = E[ ∑t=0T γt ( R(Xt, π(Xt)) - L(Xt, π(Xt)) ) ],

for 0 < γ ≤ 1, with reward function R and loss function L defined by the full market model.

Conjecture (Existence of optimal strategy). Under natural regularity conditions on:

the hybrid state dynamics (well-posed flows and jump maps on M × S),
the action sets A(x) (compact, measurable),
and the reward / loss structure (bounded or suitably integrable, Markov in (Xt, at)),
there exists an optimal admissible policy π* such that

J(π*) = ⊃π J(π),

and π* can be chosen from a structured class, for example:

a Markov / feedback policy π*(x) depending only on the current state, or
a variational / geometric policy that realizes a least-action trajectory on the manifold (solution of an HJB / Euler–Lagrange system associated with the full model).
A stronger version asks for constructive existence: that π* can be obtained as the limit of a convergent sequence of algorithms (e.g. value iteration, policy iteration, or actor–critic methods) implemented within the same market simulator.

Multi-agent / equilibrium extension
In the many-agent setting, each agent i has its own objective Ji(π1, …, πN) defined over the same hybrid geometric field, with coupling via prices, fields, and information graphs.

Extended Conjecture (Existence of geometric–informational equilibrium).
For the full multi-agent model (or for its mean-field limit), under suitable compactness / continuity / monotonicity assumptions, there exists at least one equilibrium strategy profile:

a Nash equilibrium in feedback strategies, or
a mean-field game equilibrium (solution of a coupled HJB–Fokker–Planck system on the manifold),
compatible with the hybrid dynamics and information network.

What counts as a solution
Any of the following would constitute a serious solution to this conjecture:

An existence theorem (single agent): precise assumptions + proof that an optimal feedback policy π* exists for the full hybrid geometric–informational model (beyond the finite, discretized MDP case).
A constructive characterization: a scheme (e.g. an HJB on M × S or a convergent dynamic-programming / RL algorithm) that provably converges to π*.
An equilibrium existence result (multi-agent / mean-field): proof that, in the interacting-agent version, at least one Nash / mean-field equilibrium exists in feedback / geometric strategies.
Bonus: Beyond proving that an optimal strategy exists, explicit constructions, structural characterisations (e.g. geodesic, layered across time-scales), or practically implementable algorithms that find it will be given special recognition.
