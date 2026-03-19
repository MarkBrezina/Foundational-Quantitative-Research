# Framework

## Overview

Financial markets as a **multi-layered dynamical system**.

At each time step, the system evolves through interactions between:

1. Fundamentally **Assets and Markets (structure)** as visualised/represented by a first a graph network over markets, with components being assets in those markets.
Markets are thought of *simply* as exchanges, with say stocks being traded, bonds or forex.

2. **Agents (decision-makers)** as traders, institutions, market-makers and so on. Each agent has a breadth of actions available to them, each onto the assets and markets represented below.

3. **Rules (constraints and dynamics)** To the common financially literate individual this is obvious, rules may be "opening hours", they may be market psychology, they may be statistical in nature. We do not intend to seem as if we understand all of them or believe our selves to be all knowing. This will indeed be the main part of our research further on.

4. **Valuation Layers (state of the system)** - This is where all of the aggregations of the previous layers show up as the "price movement" or "price position" of assets and this is what we will in the end see on the stock market price.

---

## 1. Assets and Markets Graph

Assets are represented as nodes in a graph:

- Equities, derivatives, indices, crypto assets
- Exchanges and trading venues
- Structural relationships (index membership, derivatives)

Edges encode:

- Correlation and dependency
- Structural links (ETF → constituents)
- Market relationships (cross-listings)

This graph allows modelling of:
- Shock propagation
- Contagion
- Cross-asset dynamics

---

## 2. Agents Graph

Agents represent:

- Traders
- Institutions
- Strategies
- Algorithms

Edges represent:

- Information sharing
- Competition
- Influence

Each agent:
- Has a state (positions, capital, beliefs)
- Optimises a utility function
- Acts based on rules and information

This enables:
- Herding dynamics
- Strategy interaction
- Emergent behaviour

---

## 3. Rule-Based System

The system evolves through hierarchical rules:

### Tier 1 — Constraints
- Capital limits
- Risk constraints
- Feasibility (no-arbitrage, conservation)

### Tier 2 — Dynamics
- Stochastic evolution
- Regime changes
- Information arrival

### Tier 3 — Interactions
- Trading
- Price formation
- Agent interaction

Rules define the **mechanics of the market**.

---

## 4. Multi-Layer Value Surface

Each asset has multiple “values”:

- **Fundamental value**  
- **Financially adjusted value**  
- **Information-adjusted value**  
- **Interaction / market price**

These layers evolve over time and interact.

The differences between layers capture:
- Mispricing
- Sentiment
- Liquidity effects

This is a core innovation of the framework :contentReference[oaicite:0]{index=0}.

---

## 5. Dynamics

The system evolves in discrete time:

1. Update information and state
2. Propagate values across the graph
3. Agents act and interact
4. Prices update via supply-demand
5. Feedback loops influence next step

This creates a **closed-loop system**.

---

## 6. Decision Layer (Control Perspective)

Agents solve a **risk-aware control problem**:

- Maximise reward – loss
- Subject to constraints
- Using predictions and risk models

This aligns with Bellman-style optimisation and control theory :contentReference[oaicite:1]{index=1}.

---

## 7. Key Properties

This framework enables:

- Multi-scale modelling (micro → macro)
- Explicit feedback loops
- Integration of structure and dynamics
- Simulation of realistic phenomena:
  - Contagion
  - Volatility clustering
  - Regime shifts

---

## 8. Implementation Mapping

| Concept | Code Module |
|--------|------------|
| Asset Graph | `market_graph/` |
| Agents | `agents/` |
| Rules | `rules/` |
| Valuation | `valuation/` |
| Simulation | `simulation/` |
| Execution | `execution/` |

---

## Summary

This framework provides a **unified architecture** for:

- Understanding markets
- Simulating behaviour
- Designing strategies
- Managing risk

It is intended as both a **research tool** and a **practical foundation for systematic trading systems**.
