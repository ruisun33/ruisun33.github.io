# TRACE visual

Redrawn from Figure 1 and Sections 2–3 of the [TRACE paper](https://arxiv.org/html/2609.10315).
The taller layout makes the labels larger at the same website width.

The three main stages preserve the simulator, solvability verifier, and agent
interface. Hidden labels go into reward and scoring, not the agent's observations.
SFT supplies the warm start before RL. Segment attribution applies when required
by the cause type; supporting evidence is not itself scored in the reward.

Edit trace.drawio and export to assets/img/research/trace.svg.
The original Figure 1 remains in assets/img/research/trace-workflow.png.

## Wide alternative

trace-wide.drawio and assets/img/research/trace-wide.svg preserve all
text from the original layout on a 1760 × 625 canvas. Training and evaluation
sit in a right-hand column. The research page currently uses this wider version;
trace.drawio and trace.svg remain unchanged for comparison.
