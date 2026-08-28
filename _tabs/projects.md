---
title: Projects
icon: fas fa-cubes
order: 1
permalink: /projects/
---

My work sits at the intersection of **reinforcement learning, evaluation, and sequential
decision-making**. The common thread is *signal*: where a learning signal comes from, how
much of it a domain can supply, and what you can build when the domain supplies none.

## Trace-Bench

**A benchmark and RL training substrate for causal diagnostic reasoning.**
{: .mb-1 }

Root-cause analysis is a workflow shared across advertising, finance, supply chain, and
healthcare operations: a metric moves, and someone has to work out *why*. Production data
can never give you ground truth for this — the counterfactual isn't observable. Trace-Bench
sidesteps that by generating **6,000 synthetic episodes** in which a known causal
intervention is embedded inside realistic, high-dimensional time-series data, then giving
the agent Python and SQL tools to explore it. Because the intervention is known by
construction, every trajectory can be scored automatically.

That scoring function doubles as a reward function, which turns the benchmark into a
training substrate: RL fine-tuning took **Qwen3-32B from 20% → 70% pass@1**, reaching near
parity with frontier models on held-out tasks. The broader claim is that verifiable reward
signals can be *constructed* through domain simulators, rather than borrowed from the few
domains that happen to have natural verifiers.

*Preprint in preparation.* &nbsp;[Read more →](/posts/trace-bench/)
{: .project-meta }

## Dynamic Context Management

**Adaptive context selection via primal-dual Thompson sampling.**
{: .mb-1 }

Context windows grew from 4K to 128K+, but what goes into them is still decided by static
heuristics — recency truncation, front truncation, summarization at a fixed ratio. This
work reframes context management as a **constrained resource allocation problem** and
solves it with **Primal-Dual Thompson Sampling (PD-TS)**, which learns online which context
policy to apply while respecting a token budget. It beats fixed heuristics at equivalent
token budgets on multi-hop QA.

This is the strand of earlier work on bandits and primal-dual methods that transferred most
directly to LLM systems.

*Paper in progress.* &nbsp;[Read more →](/posts/dynamic-context-control/)
{: .project-meta }

## Ad Arena

**A simulation environment and LLM benchmark for search advertising bidding.**
{: .mb-1 }

Search advertising is a sequential decision problem under partial, noisy feedback: which
keywords to bid on, how much, what copy to show, how to pace budget — repeated daily
against competitors in second-price auctions. Ad Arena provides a controlled simulator for
this, extended into a public LLM benchmark with a leaderboard that pits language models
against baseline bidding strategies. It measures not only final performance but how
*quickly* an agent learns to optimize, which is the part fixed-score benchmarks miss.

[Read more →](/posts/ad-arena/)
{: .project-meta }

## Earlier work

Peer-reviewed work in online learning and sequential decision-making — multi-armed bandits,
primal-dual algorithms, and their application to revenue management and pricing — is listed
on [Google Scholar](https://scholar.google.com/citations?user=83Q2zz4AAAAJ).
