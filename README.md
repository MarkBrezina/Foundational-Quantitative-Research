# Foundational Quantitative Research (FQR)

> This repository documents a framework-first research approach to financial markets.  
> It is not investment advice, not a trading signal repository, and not production execution infrastructure.
> I, like anyone else underneath the sky, use AI, not for coding, thinking, or buidling. But for improving writing and understanding.

## Disclaimer

This repository is **not** intended to showcase proprietary strategies, hidden alpha, or deployable trading systems.

It exists for a different reason:

- to document a long-running research process,
- to make the underlying thinking visible,
- to organise a conceptual framework for studying markets,
- and to provide a foundation for further empirical and theoretical work.

So, to be clear:

- you will find **no proprietary alpha** here,
- you will find **no production trading code** here,
- and you will find **no “get rich quick” machinery** here.

What you *will* find is a structured walkthrough of how I think about financial markets: their structure, dynamics, valuation, interactions, and the open problems that still seem worth taking seriously.

---
## What is this?

**Foundational Quantitative Research (FQR)** is a research repository for a **framework-first** approach to financial markets.

It is built around a few central ideas:

- **Market structure as graphs**  
  Assets, venues, flows, and agents represented as interacting structures rather than isolated time series.

- **Layered valuation surfaces**  
  Multiple notions of “price” evolving under different rules, frictions, and informational constraints.

- **Simulation and empirical validation**  
  A research process that moves back and forth between conceptual models, simulations, and real-world data.

- **Conjectures as open research programs**  
  A set of explicit problems, each paired with baseline implementations, metrics, and possible empirical tests.

- **Foundational components for broader research**  
  Parts of a larger long-term framework for modelling financial systems more coherently.

This repository is the **foundation layer** for a broader markets research program: defining primitives, building reusable tooling, and studying questions across microstructure, execution, valuation, regimes, and market interaction.
---
## Why this repository exists

The goal of this repository is to make it easier to:

1. **run experiments end-to-end**,  
2. **reproduce figures, results, and baselines**,  
3. **iterate on open problems in a structured way**,  
4. **document a coherent foundation for future work**,  
5. **show that the ideas are not just vague abstractions, but researchable objects**.

This is, above all, a research repository.

It is meant to show the *walkthrough of the thinking*.

---

## What you will find here

The repository is organised around three broad layers:

### 1. Framework
The conceptual and structural lens used to think about markets:
- assets, venues, and agents,
- rules and interactions,
- valuation and dynamics,
- abstractions that connect market behaviour across scales.

### 2. Empirical studies
Reproducible experiments on real or simulated data:
- cross-asset and cross-venue relationships,
- structural breaks and market regimes,
- flow, volatility, and impact proxies,
- validation of proposed mechanisms.

### 3. Conjectures
Open research problems treated as **mini research programs**:
- clearly stated ideas,
- motivation for why they matter,
- baseline implementations,
- metrics and falsifiable criteria,
- links to experiments, notes, and formal writeups.

---

## Recommended reading path

This repository is designed to be read in layers.

If you want the intended path, follow this order:

### Step 1 — Start here
Read this frontpage first:

- [`README.md`](README.md)

This should give you the high-level picture: what the repo is, what it is not, and how to navigate it.

### Step 2 — Get the map
Then go to:

- [`docs/overview.md`](docs/overview.md)

This is the broad overview of the landscape.  
Big ideas first. Minimal technical overhead.

### Step 3 — Read the framework
Then continue to:

- [`docs/framework/README.md`](docs/framework/README.md)

This is where the architecture of the framework starts becoming concrete.

From there, move into the components:

- `docs/framework/assets.md`
- `docs/framework/agents.md`
- `docs/framework/rules.md`
- `docs/framework/valuation.md`
- `docs/framework/dynamics.md`

### Step 4 — Read the conjectures
Once the framework makes sense, continue to:

- [`docs/conjectures/README.md`](docs/conjectures/README.md)

This introduces the open problems and explains why they matter.

### Step 5 — Go deeper where needed
For each conjecture or topic, move into the dedicated folders for more serious detail:

- `conjectures/conjecture_X/README.md` for intuition and structure
- `math.md` for formalism
- notebooks or experiments for concrete work
- `papers/` for longer-form writeups and technical drafts

---

## Reading philosophy

A rough guide to the repo is:

```
README.md
   ↓
docs/overview.md
   ↓
docs/framework/
   ↓
docs/conjectures/
   ↓
conjectures/conjecture_X/
   ↓
papers/
   ↓
experiments/ + src/
```

*As taken from [TMRW Conjectures](https://tomorrowcapitalresearch.github.io/Conjectures.html)

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

```

