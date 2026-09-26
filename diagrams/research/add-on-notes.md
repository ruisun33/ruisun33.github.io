# Add-on discount visual

Editable source: `add-on.drawio`. Website export: `assets/img/research/add-on.svg` (1400 × 744).

Sources:
- Published paper: https://pubsonline.informs.org/doi/10.1287/mnsc.2021.4222
- Full preprint: https://arxiv.org/pdf/2005.00947

The top row is a schematic, not an experimental result. A core purchase unlocks discounts on selected complementary products; customers choose add-ons individually. The method jointly chooses regular prices, discount prices, and which supportive products receive discount offers. The number of offers is constrained; the illustration does not imply an inventory constraint.

UCB-Add-On maintains optimistic demand estimates and passes them to an FPTAS for joint pricing and offer optimization. Purchase observations update those estimates. The diagram abstracts away the episode schedule and decreasing approximation tolerance. The FPTAS gives a (1−epsilon) approximation for known demand; the learning method has a sublinear regret guarantee against the optimal policy with known demand, under the paper's assumptions.

Tmall evidence uses simulations calibrated to historical transaction data. Discount response effects are assumed in the experiments because historical data did not include add-on discounts. This is not a live randomized deployment or a measured causal uplift claim.

The wide version uses a 1760 × 625 canvas with a compact problem illustration
on the left and three method cards on the right. Text is reflowed and some
labels shortened; shapes and typography use natural proportions.

Latest wide layout: the problem and method occupy balanced left/right columns.
The three method steps are stacked vertically; the canvas remains 1760 × 625.

Spacing refinement: the left illustration is narrower (688px), with its heading
aligned above the panel. The method occupies 912px; both start and end together.
