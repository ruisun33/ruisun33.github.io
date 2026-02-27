---
title: "Trace-Bench"
date: 2025-02-25
categories: [projects]
tags: [benchmark, reinforcement-learning, agentic-ai]
pin: false
---

> A benchmark and RL training substrate for evaluating causal diagnostic reasoning in AI agents
{: .prompt-info}

## TL;DR

Ad-Agentic-Bench is a benchmark of **6,000 synthetic episodes** that evaluates an AI agent's ability to perform root cause analysis through iterative exploration using Python tools with SQL support. Each episode embeds a known causal intervention inside realistic, high-dimensional time-series data — giving you something you can never get from production data: **unambiguous ground truth**.

It doubles as a training substrate for reinforcement learning, turning evaluation trajectories into reward signals that measurably improve agent reasoning.

## The Problem

Analysts across advertising, finance, supply chain, and healthcare operations share a common workflow: a key metric moves, and they need to figure out *why*. This means exploring complex multi-table data, forming hypotheses, and separating true causes from correlated noise.

As agentic AI systems get deployed to automate this, we need benchmarks that test the **full diagnostic reasoning loop** — not just SQL proficiency or tool use in isolation. Existing benchmarks don't require multi-step causal reasoning over high-dimensional time-series data.

## How It Works

### Data Generation

A stochastic simulator produces each episode in three layers:

1. **Realistic campaign data** across a 9-dimensional segment hierarchy (advertiser, campaign, ad group, keyword, match type, device, geo, time, platform)
2. **Controlled causal intervention** — one of 8 cause types with calibrated effect sizes (20–55%)
3. **Confounders** — budget fluctuations, inventory changes, and platform shifts that the agent must distinguish from the true signal

### The Agent's Task

Given an episode, the agent must:
- Explore the data using Python tools with SQL support
- Form and test hypotheses iteratively
- Identify the root cause of a metric change
- Distinguish it from decoy signals (confounders)

### Difficulty Tiers

Three progressive difficulty levels that systematically withhold temporal and contextual hints, testing how robust the agent's reasoning is when it can't rely on shortcuts.

## Why Advertising?

Digital advertising is the instantiation domain because it exhibits the full complexity of enterprise diagnostic tasks:

- **High dimensionality** — campaigns span many segment combinations
- **Temporal non-stationarity** — performance shifts over time for legitimate reasons
- **Multiple simultaneous decoy signals** — budget changes, seasonality, and platform updates all look like plausible causes

The patterns generalize to any domain where analysts diagnose metric changes in complex data.

## Key Insight: Synthetic Ground Truth

Real operational data can never tell you the true cause of a metric change with certainty. The synthetic design solves this — every episode has an unambiguous causal label, enabling:

- **Fully automated evaluation** — no human annotation needed
- **RL reward signals** — agent trajectories become training data
- **Systematic error analysis** — you can categorize exactly *how* models fail

## Evaluation

Three frontier models benchmarked: **Claude Opus 4**, **Claude Sonnet 4**, and **Qwen3-235B**.

Error analysis identifies four systematic failure modes:
- **Confounder confusion** — mistaking a decoy signal for the true cause
- **Incomplete segment exploration** — not drilling deep enough into the hierarchy
- **Temporal misalignment** — attributing the change to the wrong time window
- **Cause-label misattribution** — correct root cause identification but wrong categorization

## Dual Purpose: Benchmark + Training Substrate

Ad-Agentic-Bench isn't just for evaluation. RL training on benchmark trajectories improves agent diagnostic reasoning, validating its use as a training substrate — not just a leaderboard.

## Open Source

Fully open-sourced:
- Data generation simulator
- Evaluation harness
- Baseline trajectories

---

*Paper in progress. Check back for the preprint link.*
