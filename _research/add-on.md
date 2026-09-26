---
title: "Online Learning with Add-on Discounts"
order: 3
image: "/assets/img/research/add-on-wide.svg"
image_alt: "A core purchase unlocks selected add-on discounts. UCB demand estimates feed an FPTAS that jointly chooses prices and discount offers; observed purchases update demand estimates."
image_wide: true
image_width: 1670
image_height: 625
keywords: ["Online Learning", "Multi-Armed Bandit", "Dynamic Pricing"]
paper: "https://pubsonline.informs.org/doi/10.1287/mnsc.2021.4222"
---

Pricing becomes substantially harder when products interact. A retailer selling a core product may also offer discounted complementary items. **This couples several decisions**: which products should be discounted, how large should the discounts be, and how should the core product itself be priced? These decisions become more challenging when the relationship between prices and customer demand is unknown.

We address this problem by combining **combinatorial optimization with online learning**. An approximation algorithm makes the large pricing and assortment space computationally tractable, and an upper-confidence-bound strategy uses observed customer responses to learn unknown demand over time. Together, they yield an efficient learning algorithm with performance guarantees. Experiments using transaction data from a Tmall retailer further demonstrate the value of jointly optimizing prices and add-on discounts.