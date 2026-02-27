---
icon: fas fa-project-diagram
order: 1
---

A collection of projects I'm currently working on — spanning causal reasoning benchmarks, efficient inference, and simulation environments for ad systems.

---

### [Ad-Agentic-Bench](/projects/ad-agentic-bench/)
A benchmark and RL training substrate for evaluating causal diagnostic reasoning in AI agents. 6,000 synthetic episodes with ground-truth causal labels, testing whether agents can perform root cause analysis through iterative data exploration — distinguishing true causes from confounders in high-dimensional time-series data.

### [Dynamic Context Window Management](/projects/dynamic-context-window/)
Adaptive context management for LLMs via Primal-Dual Thompson Sampling. Reframes the "what to keep in the context window" problem as constrained resource allocation under uncertainty — jointly learning policy utility and enforcing token budgets. Outperforms static heuristics on multi-hop QA at equivalent cost.

### [Ad Auction Gym](/projects/ad-auction-gym/)
A simulation environment for search advertising auctions, extended into a public LLM benchmark with a web leaderboard. Measures not just final bidding performance but how quickly agents learn to optimize — tracking strategy volatility, convergence, and learning rate across standardized scenarios.
