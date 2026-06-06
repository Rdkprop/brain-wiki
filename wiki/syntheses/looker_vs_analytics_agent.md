---
title: Looker vs Analytics Agent — Interface Ownership Strategy
type: synthesis
domain: work
created: 2026-06-06
updated: 2026-06-06
related_pages:
  - analytics_agent
  - pg_consumer_metrics
  - propertyguru
confidence: high
sources:
  - raw/daily/Friday June 5th.md
tags: [looker, analytics, agent, strategy, work, propertyguru]
---

# Looker vs Analytics Agent — Interface Ownership Strategy

A POV on the ongoing debate (surfaced by Vasu) about whether Looker Explores are still needed once an Analytics Agent exists. The core question is not about dashboards — it's about **who owns the exploration interface**.

---

## The Real Question

Vasu's push is not about replacing SOT dashboards (those are settled). It's about whether **Looker Explores** — the self-serve analysis layer — are still needed once the [[Analytics Agent]] matures.

Two options on the table:

| Option | Stack |
|--------|-------|
| **A (current)** | Dashboards + Explores + analyst-led exploration |
| **B (emerging)** | Dashboards + Agent as exploration interface; Explores deprecated |

His direction: *"The agent is where exploration happens."*

---

## The Concern Underneath

If both Explores and the Agent co-exist permanently:
- Users split across interfaces
- Adoption dilutes ("Why go to Looker?" becomes a recurring question)
- Org complexity increases without clear benefit

This is why convergence is being pushed.

---

## The Practical Constraint (Current Reality)

The [[Analytics Agent]] is promising but **not yet reliable enough to fully replace Explores**. Current gaps: occasional instability, connectivity issues, routing failures. Dashboards are stable — the agent is not at the same reliability level yet.

**Therefore:** replacement is not ready, not because dashboards fail, but because the agent hasn't earned full trust as an exploration interface.

---

## The 3-Phase Transition Plan

### Phase 1 — Stabilise roles, keep both (now)

| Layer | Role |
|-------|------|
| Looker dashboards | SOT reporting |
| Looker Explores | Power users + fallback |
| Analytics Agent | Assisted exploration (not primary yet) |

**Principle:** Agent complements, doesn't replace.

### Phase 2 — Make agent the default entry point

Behavioural goal: get teams to start with the agent first.
- "Ask the agent first"
- Explores become the **escape hatch**, not the starting point

This reduces reliance on Explores gradually without breaking trust.

### Phase 3 — Replace Explores only when agent meets criteria

Deprecate Explores only when the agent demonstrably meets:
- Stability
- Accuracy
- Reliability
- Sufficient coverage across real use cases

---

## The Strategic Framing (3 Layers)

To avoid confusion in internal conversations, frame the future stack as:

1. **Metrics layer** — governed definitions in BigQuery
2. **Consumption layer** — dashboards + agent as the interactive interface
3. **Fallback layer** — Looker Explores (temporary, being phased out)

This aligns with Vasu's convergence direction while respecting current reliability constraints. It avoids a premature deprecation that would damage trust in the agent.

---

## Interface Ownership Insight

This is not just a tooling decision — it's an **interface ownership** decision:
- Whoever controls the exploration interface controls adoption and standardisation
- Today: Looker UI owns exploration
- Future direction: Agent interface

The goal is to move that ownership gradually toward the agent without prematurely removing a tool people still rely on for correctness.
