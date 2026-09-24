---
title: "Bandit Learning for Online Matching"
order: 4
image: "/assets/img/research/bandits.svg"
image_alt: "Bandit Learning for Online Matching visual placeholder"
keywords: ["Bayesian Learning", "Online Matching", "Bandits"]
paper: "https://pubsonline.informs.org/doi/10.1287/opre.2021.0499"
---

How should a platform allocate scarce resources when the value of each match is initially uncertain? **Every allocation serves two purposes:** it generates an immediate reward, but also reveals information that can improve future decisions. Learning therefore competes directly with resource conservation—exploration itself consumes the limited capacity we are trying to allocate efficiently.

We formulate this problem as a **budget-constrained Bayesian bandit**. Our approach first constructs policies for individual arms through a relaxed optimization problem, then coordinates their selection and execution under shared resource constraints. This decomposition turns a difficult high-dimensional adaptive decision problem into tractable subproblems while retaining a provable approximation guarantee relative to the optimal policy.
