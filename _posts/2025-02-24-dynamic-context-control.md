---
title: "Dynamic Context Management"
description: "Reframing LLM context management as constrained resource allocation, and solving it online with Primal-Dual Thompson Sampling."
date: 2025-02-24
categories: [Projects]
tags: [context-management, thompson-sampling, test-time-optimization]
pin: false
featured: 2
---

> Adaptive context management for LLMs via Primal-Dual Thompson Sampling

## TL;DR

LLMs have finite context windows, and current methods for deciding what to keep (recency truncation, summarization) are static heuristics that ignore task structure and system constraints. We reframe context management as a **constrained resource allocation problem** and propose **Primal-Dual Thompson Sampling (PD-TS)** — a learning-based approach that adaptively selects context policies while respecting token budgets. It outperforms fixed heuristics at equivalent token budgets on multi-hop QA.

## The Problem

Context windows have grown from 4K to 128K+ tokens, but managing what goes in them is still handled by blunt heuristics:

- **Recency truncation** — keep the latest messages, drop the rest
- **Front truncation** — drop the beginning
- **Summarization** — compress older content

These break down in practice because:

1. **Task structure matters** — multi-hop reasoning needs entity mentions scattered throughout the context, not just recent ones
2. **Position effects** — the "lost in the middle" phenomenon means information placement affects retrieval quality
3. **System constraints** — longer prompts cost more and add latency
4. **More isn't always better** — attention dilution and distractor effects mean stuffing the window can hurt performance

## Key Insight

Context window management is analogous to **network revenue management**: each inference call must allocate a limited token budget among competing pieces of information whose utility is initially unknown.

This is a resource allocation problem under uncertainty — and there's a rich toolkit for that.

## Approach: Primal-Dual Thompson Sampling

PD-TS has two components working together:

**Primal (Thompson Sampling):** Learns which context management policies perform best by maintaining posterior beliefs over policy utility and sampling from them — balancing exploration and exploitation naturally.

**Dual (Lagrangian Relaxation):** Enforces average token budget constraints. The dual variable acts as a "price" on tokens, penalizing policies that consume too much budget and steering selection toward efficient ones.

The combination gives you adaptive policy selection that respects hard context limits and soft budget targets simultaneously.

## Formalization

The problem is framed as episodic decision-making with:
- A **hard context limit** — the model's maximum window size
- A **soft token budget** — an average cost constraint across episodes
- **Unknown policy utilities** — learned online through interaction

Each episode, PD-TS selects a context management policy (e.g., "summarize + keep last 3 turns" vs. "retrieve top-k relevant chunks"), observes the outcome, and updates both the utility estimates and the budget dual variable.

## Evaluation

Evaluated on multi-hop QA benchmarks using an offline replay protocol. PD-TS outperforms fixed heuristics at equivalent token budgets — getting better answers while spending the same number of tokens.

---

*Paper in progress. Check back for the preprint link.*
