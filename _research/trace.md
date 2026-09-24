---
title: "TRACE: Training Reasoning Agents with Synthesized Rewards"
order: 1
image: "/assets/img/research/trace-workflow.png"
image_alt: "TRACE workflow: a stochastic simulator generates diagnostic tasks, an oracle verifies the evidence, and the task interface supports RL training and evaluation."
image_wide: true
image_width: 1582
image_height: 442
image_source: "https://arxiv.org/abs/2609.10315"
keywords: ["Agentic Post-Training", "Reinforcement Learning", "Simulation"]
paper: "https://arxiv.org/abs/2609.10315"
code: coming-soon
huggingface: coming-soon
---

Reinforcement learning with verifiable rewards has been highly effective in domains such as mathematics and coding, where checking an answer is easier than finding one, a property also known as the **asymmetry of verification**. However, many real-world reasoning tasks do not offer such inexpensive ground-truth verification.

TRACE explores whether we can engineer this asymmetry through simulation: hidden interventions generate noisy, confounded observations, making diagnosis challenging while keeping the underlying cause known and the reward objectively verifiable. On our held-out benchmark, supervised fine-tuning combined with reinforcement learning improves Qwen3.5-35B-A3B, outperforming all evaluated prompted baselines, including Claude Opus 5. Our results suggest that **scalable, objective training signals** can be as important as model scale for building capable reasoning agents.
