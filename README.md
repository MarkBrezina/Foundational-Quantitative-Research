# Foundational Quantitative Research (FQR)

A research repository for a **framework-first** approach to financial markets:
- **Market structure as graphs** (assets/venues + agents)
- **Layered valuation surfaces** (multiple “prices” evolving under rules)
- **Simulation + empirical validation**
- A set of open research problems (“conjectures”) with reproducible baselines

> This is research software. It is **not** trading advice, and nothing here should be interpreted as a recommendation to buy/sell any asset.

---

## What this repo is

This repository is the “foundation layer” for a broader markets research program: defining primitives, building a reusable simulation engine, and running empirical studies (correlations, venues, regimes, microstructure).

The goal is to make it easy to:
1) **Run experiments end-to-end**,  
2) **Reproduce plots/results**, and  
3) **Iterate on open problems** via testable baselines.

If you’re looking for: *one-off notebooks*, *a trading bot*, or *production execution code*, this is not that repo.

---

## Research tracks

### 1) Framework simulations
A minimal but extensible simulator that supports:
- Market graph construction (assets/venues)
- Agent populations + policies
- A rule hierarchy (invariants → stochastic dynamics → interactions)
- Time evolution + metrics

### 2) Empirical studies
Reproducible experiments on real data:
- Cross-asset and cross-venue correlation networks
- Structural breaks and regime clustering
- Flow/vol/impact proxies and validation tests

### 3) Conjectures (open problems)
Each conjecture is treated as a **mini research program**:
- A clear statement
- A baseline implementation
- Metrics and falsifiable criteria
- Reproduction steps

Current set (see `/docs/conjectures/` and `/conjectures/`):
- Conjecture 1 — Path switching
- Conjecture 2 — Central pricing
- Conjecture 3 — Stealth / optimal distributions
- Conjecture 4 — Full market model

---

## Repository layout

```text
.
├── README.md
├── LICENSE
├── CITATION.cff
├── CONTRIBUTING.md
├── CHANGELOG.md
├── docs/
│   ├── overview.md
│   ├── framework/
│   └── conjectures/
├── papers/
│   ├── Whitepaper - Financial markets model.pdf
│   ├── 2nd initial draft.pdf
│   └── Conjecture *.pdf
├── src/
│   └── fqr/
│       ├── market_graph/
│       ├── agents/
│       ├── rules/
│       ├── valuation/
│       ├── simulation/
│       └── execution/
├── experiments/
│   ├── 00_data_sanity/
│   ├── 01_assets_and_venues/
│   ├── 02_correlation_networks/
│   ├── 03_regimes_and_features/
│   └── 04_agent_sims/
├── conjectures/
│   ├── conjecture_1_path_switching/
│   ├── conjecture_2_central_pricing/
│   ├── conjecture_3_stealth_execution/
│   └── conjecture_4_full_model_optimality/
├── notebooks/          # demos only (no core logic)
├── data/               # usually gitignored (see data/README.md)
└── results/            # gitignored (outputs written here)
