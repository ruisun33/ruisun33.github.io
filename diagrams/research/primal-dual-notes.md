# Primal-dual research visual

Source: [Near-Optimal Primal-Dual Algorithms for Quantity-Based Network Revenue Management, PDF v4](https://arxiv.org/pdf/2011.06327v4), Sun, Wang, and Zhou. All 62 pages, including appendices A–C, were read for this visual.

The illustration explains the core bid-price loop (Section 3, Algorithm 2).
The request uses one unit of each of two illustrative resources. Bars are schematic,
not experimental data. Resource values are internal bid prices, not customer prices.
Gradient updates compare consumption with the per-period resource budget.

The right-hand panel illustrates learned admission thresholding (Section 4)
from the basic loop. Reward is used as a general interpretation of the paper's revenue
objective; this does not extend its guarantees to arbitrary reward models.
The right-hand panel explains near-optimality through the relative gap to the hindsight planner.
The acceptance scale and its cutoffs are schematic, not estimated data.
Acceptance is normalized by expected arrivals of each request type in the early
phase, matching Algorithm 3's comparisons with arrival-rate-scaled thresholds.
System scaling means demand and resource capacity grow proportionally.
The PDF proves the stronger diffusion-scale guarantee under its stated
assumptions. No numerical bound is displayed: the abstract landing page and PDF
currently report different bounds. Hybrid variants in Appendix C can solve LPs;
the illustration concerns the paper's LP-free algorithms.

Edit primal-dual.drawio in diagrams.net, then export SVG to
assets/img/research/primal-dual.svg. The current SVG also embeds the editable
draw.io source. Preserve that option when exporting if desired.

The page now previews primal-dual-wide.svg, exported from
primal-dual-wide.drawio. It gives the threshold panel more width.
The prior primal-dual-wide.drawio and SVG are retained for comparison.

The page has been switched back to primal-dual-wide.svg by preference.
The balanced alternative remains available for comparison.
