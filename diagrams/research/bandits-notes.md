# Bandit matching visual

Source: https://pubsonline.informs.org/doi/10.1287/opre.2021.0499

Editable source: `bandits.drawio`; website export: `assets/img/research/bandits.svg` (1400 × 755).

This conceptual overview follows the published abstract and the site's research description. The full paper was not available through the accessible publisher page; no experimental results or detailed policy construction are depicted.

Matching rewards depend on resource type and arrival period. Reward distributions have known priors; observations reveal information about the unknown rewards. Each arm represents a resource-period pair, not merely a resource. A relaxed linear program supplies single-arm policies; selection and pulling order coordinate those policies subject to resource capacities.

The capacity blocks and arrival icons are schematic, not data. The upper feedback loop explains learning during matching; the lower row explains the algorithm's construction, not an instruction to solve the relaxation after every observation.

The published abstract gives an expected-reward approximation factor of (sqrt(2)−1)/2 against the optimal algorithm. The guarantee footer was removed from the visual to keep its focus on the matching problem and algorithm.

The wide version uses a 1760 × 625 canvas with a compact problem illustration
on the left and three method cards on the right. Text is reflowed and some
labels shortened; shapes and typography use natural proportions.

Latest wide layout: the problem and method occupy balanced left/right columns.
The three method steps are stacked vertically; the canvas remains 1760 × 625.

Spacing refinement: the left illustration is narrower (688px), with its heading
aligned above the panel. The method occupies 912px; both start and end together.
