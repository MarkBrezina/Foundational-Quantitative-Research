A brief restatement of the main idea:

This repository presents a **Foundational Quantitative Research (FQR)** framework for modelling financial markets as **multi-layered, adaptive systems**.

Rather than treating markets as isolated time series or static statistical objects, this framework treats them as **complex dynamic systems** shaped by structure, interaction, rules, and evolving valuation.

You can think of markets as involving:

- **Networks of assets and markets**  
  Relationships between assets, venues, and sectors evolve over time. Correlations shift, dependencies appear and disappear, and local effects can propagate across scales.

- **Networks of interacting agents**  
  Different groups of participants act under different objectives, constraints, and information sets. Some are systematic, some discretionary, some coordinated, some independent, and information is rarely shared symmetrically.

- **Rule-driven dynamical systems**  
  Markets evolve under layered rules: matching rules, inventory constraints, liquidity constraints, behavioural responses, institutional frictions, and broader structural dynamics.

- **Layered valuation surfaces evolving through time**  
  Price is not treated as a single primitive object, but as something that emerges from the interaction between agents, assets, rules, and information. What we observe as price is the surface-level consequence of deeper system behaviour.

This framework aims to bridge several levels of analysis:

- **Market microstructure**  
  Order flow, liquidity, execution, and short-horizon price formation.

- **Medium-frequency behaviour**  
  Trends, reversals, crowding, delayed responses to news, and behavioural effects.

- **Macro dynamics**  
  Regimes, fundamentals, systemic interactions, and large-scale market shifts.

- **Agent behaviour**  
  Strategies, learning, adaptation, incentives, and interaction.

---

## A simple intuition

### 1. Markets
Financial markets are places where participants buy, sell, and propose prices at which they are willing to transact. Someone wants to buy at one price, someone else wants to sell at another, and some mechanism matches them.

This basic structure appears in many forms:
- auction houses,
- farmers' markets,
- marketplaces,
- stock exchanges,
- cryptocurrency exchanges,
- and many other trading environments.

The exact mechanism may differ, but the essential idea remains the same:
**someone is willing to buy at price X, someone is willing to sell at price Y, and price discovery happens through interaction.**

That was part of what made markets interesting to me in the first place. In many ordinary settings, prices are simply posted and accepted. In markets, however, behaviour, psychology, and competition help determine outcomes. That makes them not only economically important, but intellectually fascinating.

### 2. Assets and objects of trade
Markets contain assets, commodities, or other tradable objects. These can be exchanged, repriced, accumulated, and reinterpreted over time.

Sometimes these objects have an obvious or widely accepted intrinsic or fundamental value. Gold is a useful example: while its valuation is still shaped by markets, many people treat it as having durable and recognisable value, especially in uncertain periods.

Other assets are more difficult to anchor. Cryptocurrencies are a good example. For many people, they remain harder to value in traditional terms, which makes them especially interesting as a research problem. Their behaviour forces us to confront how valuation emerges when “fundamental value” is ambiguous, contested, or structurally weak.

### 3. Agents
Markets also contain participants: traders, institutions, market makers, funds, retail actors, algorithms, and other decision-making entities.

Some are humans acting with emotion, belief, fear, or conviction. Others are automated systems executing predefined logic. In either case, their behaviour is not arbitrary. It follows some mixture of:
- objectives,
- constraints,
- information,
- incentives,
- heuristics,
- and rules.

In that sense, both human and algorithmic actors can be studied as systems with behaviour.

### 4. External influence
Markets do not evolve in isolation.

News, policy, regulation, technology, macroeconomic conditions, social attention, sentiment, and broader systemic forces all affect participant behaviour. Those behavioural shifts then affect valuation, liquidity, price formation, and market structure.

So the market is not just a machine for producing prices. It is a responsive system embedded in a wider environment.

### 5. The real goal
The goal is not necessarily to know the exact future price of an asset at a specific instant.

The deeper goal is to understand the **behaviour of the system**.

In many cases, it is more valuable to understand that:
- expected conditions are improving,
- risk is compressing,
- a transition in regime is likely,
- or a path is becoming unstable,

than it is to predict that tomorrow’s return will be exactly some fixed number.

Understanding behaviour, constraints, transitions, and tendencies is often more useful than pretending to know a single exact future point.

For more on this, see the **Framework** section.  
For specific questions and open research problems, see the **Conjectures** section.

---

## Core idea

Markets and their assets should not be treated as a single process.

They are better understood as a **coupled system of interacting layers**, where:

- markets and assets form an underlying structural network that defines relationships,
- rules govern how the system can evolve,
- agents create dynamics on top of that structure,
- and valuation emerges from the interaction of all of these components.

This repository attempts to formalise that system.

---

## Why this matters

Traditional modelling approaches often isolate one part of the problem while neglecting others:

- Pure time-series models may capture statistical regularities, but often ignore structural relationships.
- Agent-based models may capture behaviour, but often lack coherent valuation machinery.
- Fundamental models may say something about long-run value, but often ignore microstructure and execution.
- Execution models may be highly detailed locally, while ignoring macro regimes and broader system behaviour.

This framework is an attempt to unify these perspectives into a **single coherent research structure** that can support:

- simulation of realistic market dynamics,
- strategy research and backtesting,
- risk-aware decision systems,
- and a more natural bridge between abstract theory and implementation.

---

## What you will find here

This repository is structured around four main components:

- **Framework**  
  The conceptual, mathematical, and structural basis of the system.

- **Conjectures**  
  Open research problems and hypotheses that guide development.

- **Experiments**  
  Empirical studies and simulation-based validation.

- **Source code (`src/fqr/`)**  
  Modular implementations of the framework’s components.

---

## Philosophy

This work sits somewhere between:

- quantitative finance,
- control theory,
- network science,
- and statistical physics.

The aim is not just to build isolated models, but to develop a **general framework for understanding markets as complex adaptive systems**.

---

## Intended use

This repository is designed for:

- quantitative researchers,
- systematic traders,
- data scientists,
- and researchers interested in complex systems.

It can be used as:

- a research reference,
- a simulation framework,
- a conceptual foundation for trading systems,
- or a starting point for further academic or applied work.

---

## Relationship to papers

Formal write-ups, derivations, and extended discussions are available in the `/papers` directory.

The documentation in this repository focuses primarily on **structure, intuition, and organisation**, while the papers provide **greater depth, formalism, and technical detail**.
