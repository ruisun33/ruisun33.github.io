---
title: "Primal-Dual Framework for Revenue Management"
order: 2
image: "/assets/img/research/primal-dual.svg"
image_alt: "Primal-Dual Framework for Revenue Management visual placeholder"
keywords: ["Optimization", "Online Algorithms", "Revenue Management"]
paper: "https://arxiv.org/abs/2011.06327"
---

Many real-world allocation problems require making decisions before future demand is known. **Accepting a request today uses capacity that may be more valuable tomorrow.** Network revenue management is a canonical example: customer requests arrive sequentially, resources are limited, and each decision must balance immediate revenue against the opportunity cost of consuming scarce capacity.

We develop a primal-dual framework that turns this global resource-allocation problem into simple online decisions guided by dynamically updated resource values. The resulting algorithms achieve near-optimal performance guarantees **without repeatedly solving large deterministic optimization problems**. More broadly, the work shows how dual variables can act as adaptive signals that coordinate sequential decisions under uncertainty.
