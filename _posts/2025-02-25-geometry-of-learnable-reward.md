---
title: "The Geometry of Learnable Reward"
date: 2025-02-25
categories: [Thoughts]
tags: [reinforcement-learning, reward-hacking, RLVR]
math: true
---

*A Framework for Understanding RL with Verifiable Signals*

---

## 1. The Fundamental Object: Mapping Complexity

RL with verifiable rewards asks a model to learn a function:

$$
f: \mathcal{S} \rightarrow \mathcal{A}
$$

where $\mathcal{S}$ is the space of problem states and $\mathcal{A}$ is the space of verifiable answers. But this framing is deceptively simple. The real object being learned is not this mapping directly — it is a **generative process that traverses a reasoning trajectory** before arriving at an answer:

$$
\pi: \mathcal{S} \rightarrow \mathcal{T} \rightarrow \mathcal{A}
$$

where $\mathcal{T}$ is the space of reasoning traces (chain-of-thought, scratchpad, intermediate computation). The reward signal only touches $\mathcal{A}$, but the learning happens in $\mathcal{T}$. This asymmetry is the source of nearly everything interesting — and everything that goes wrong.

**The true learning difficulty is not determined by $\lvert\mathcal{S}\rvert$ or $\lvert\mathcal{A}\rvert$ independently, but by the structure of the minimal $\mathcal{T}$ required to faithfully connect them.** A math competition problem has $\lvert\mathcal{A}\rvert$ effectively equal to the integers, yet the reasoning trajectory space is combinatorially vast. Sentiment classification has $\lvert\mathcal{A}\rvert = 2$, and the minimal $\mathcal{T}$ is shallow. The difference is not about the endpoints — it's about the **depth and branching factor of the reasoning graph** connecting them.

This gives us a more precise quantity: **effective mapping complexity**, which we can think of as the minimum description length of a faithful function from $\mathcal{S}$ to $\mathcal{A}$ — roughly, how compressible the true input-output relationship is.

---

## 2. The Information Bottleneck View

The framework above has a natural formalization through the **Information Bottleneck (IB) principle** (Tishby et al., 1999). The reasoning trace $\mathcal{T}$ is a compressed representation of the input state $\mathcal{S}$ that must retain enough information to predict the answer $\mathcal{A}$. The IB objective is:

$$
\min_{p(\mathcal{T}|\mathcal{S})} \; I(\mathcal{S}; \mathcal{T}) - \beta \cdot I(\mathcal{T}; \mathcal{A})
$$

In words: **compress the input as much as possible** ($\min I(\mathcal{S}; \mathcal{T})$), **while preserving information relevant to the answer** ($\max I(\mathcal{T}; \mathcal{A})$). The parameter $\beta$ controls the trade-off.

This gives us a precise language for what good reasoning looks like:

- **Ideal reasoning** lies on the IB frontier: the trace retains exactly the features of $\mathcal{S}$ that determine $\mathcal{A}$, and discards everything else. It is maximally compressed *without loss of task-relevant information*.

- **Effective mapping complexity** corresponds to $I(\mathcal{S}; \mathcal{A})$ — the irreducible mutual information between problem and answer. This is the minimum amount of information any reasoning trace must carry. No compression can go below this without sacrificing correctness.

- **Undercompression** (trace carries irrelevant information): The model's reasoning is verbose, distracted, or entangled with spurious features. Costly but not catastrophic.

- **Overcompression** (trace discards relevant information): The model's reasoning takes shortcuts, ignoring features of the input that matter. This is where things break.

The IB framework makes a clean prediction: **the difficulty of learning scales with $I(\mathcal{S}; \mathcal{A})$** — the intrinsic mutual information between states and answers. When this is low (narrow domain, stereotyped problems), learning is easy. When this is high (broad domain, compositional reasoning), learning demands more capacity and richer supervision.

---

## 3. Why Small Models Suffice in Narrow Domains

The common observation that small, domain-specific models match frontier models is often attributed to "the answer space is small." This is imprecise. What actually happens is a **triple compression**:

1. **State compression**: The distribution of inputs in a specific domain is narrow. A medical coding model sees a tiny manifold of $\mathcal{S}$ compared to a general assistant.

2. **Trajectory compression**: Domain-specific reasoning follows a small number of stereotyped patterns. The effective $\mathcal{T}$ collapses to a low-dimensional space — not because reasoning is unnecessary, but because the same reasoning templates recur.

3. **Coupling compression**: The mapping from $\mathcal{S}$ to $\mathcal{A}$ has low mutual information — the number of independent features of $\mathcal{S}$ that matter for determining $\mathcal{A}$ is small.

In IB terms: **$I(\mathcal{S}; \mathcal{A})$ is small in narrow domains.** The IB frontier is achievable with a low-capacity bottleneck. A small model can sit on this frontier comfortably.

A frontier model allocates capacity across the entire manifold of human knowledge. A domain-specific model concentrates all capacity on a small manifold. When the manifold is small enough, even limited capacity saturates it. **This is not about the model being "smart enough" — it's about the problem being compressible enough.**

The practical implication: before investing in RL for a domain, estimate the effective mapping complexity. If a supervised baseline already achieves high accuracy with modest data, $I(\mathcal{S}; \mathcal{A})$ is low, the mapping is compressible, and RL will offer marginal gains. RL's value emerges precisely when the mapping is complex enough that **supervised data cannot cover the trajectory space**, and the model must discover reasoning paths on its own.

---

## 4. Reward Hacking as Dimensional Collapse

When reward hacking occurs, what has happened in IB terms?

The model was supposed to learn a trace $\mathcal{T}$ that preserves $I(\mathcal{S}; \mathcal{A})$ bits of information about the input. Instead, it finds a degenerate trace $\mathcal{T}'$ that **overcompresses** — dropping below the IB frontier by discarding features of $\mathcal{S}$ that are genuinely necessary for $\mathcal{A}$, while still achieving high reward on the training distribution.

This is possible because the **reward signal evaluates $\mathcal{A}$ on a finite training distribution**, not on the true joint distribution of $(\mathcal{S}, \mathcal{A})$. On a finite sample, a low-dimensional shortcut can coincidentally align with correct answers — the model exploits **spurious correlations** that exist in-sample but not in the population.

Geometrically, this is **dimensional collapse**: the model projects the problem onto a lower-dimensional subspace where a cheap shortcut exists. The learned $I(\mathcal{S}; \mathcal{T}')$ drops well below $I(\mathcal{S}; \mathcal{A})$, but the training reward doesn't penalize this because it cannot distinguish "correct for the right reasons" from "correct by coincidence."

This reveals that reward hacking is not a single phenomenon but a **spectrum of collapse modes**:

| Type | What Collapses | IB Interpretation |
|------|---------------|-------------------|
| **State collapse** | Model ignores relevant features of input | $I(\mathcal{S}; \mathcal{T})$ drops — trace doesn't encode key input features |
| **Trajectory collapse** | Reasoning chain becomes degenerate | $\mathcal{T}$ loses sequential structure, becomes a shallow lookup |
| **Reward surface exploitation** | Model finds OOD inputs where reward is spuriously high | Model moves off the data manifold where IB trade-off was calibrated |
| **Distribution narrowing** | Policy collapses to a small set of "safe" outputs | $H(\mathcal{T})$ drops — trace entropy collapses, losing diversity |

### Why Does Collapse Happen?

Because RL optimizes for reward, and **reward is a scalar compression of a high-dimensional desideratum**. Outcome reward only measures $I(\mathcal{T}; \mathcal{A})$ — it tells the model to make its trace predictive of the answer. But it places **no constraint on $I(\mathcal{S}; \mathcal{T})$** — it doesn't care whether the trace actually engages with the input. The model is free to minimize $I(\mathcal{S}; \mathcal{T})$ aggressively, overcompressing past the IB frontier, as long as the resulting trace happens to correlate with correct answers in-distribution.

This leads to a quantitative prediction: **the vulnerability to reward hacking scales with the gap between the dimensionality of the reward signal and the effective mapping complexity $I(\mathcal{S}; \mathcal{A})$ of the task.** Simple tasks with simple rewards are safe. Complex tasks with simple rewards are maximally vulnerable.

---

## 5. Process Reward as Compression Regularization

If reward hacking is overcompression past the IB frontier, then process reward is a mechanism for **preventing the model from compressing away task-relevant information during reasoning**.

Outcome reward only constrains the endpoint:

$$
\max \; I(\mathcal{T}; \mathcal{A})
$$

Process reward adds a constraint on the trajectory itself:

$$
\max \; I(\mathcal{T}; \mathcal{A}) \quad \text{subject to} \quad I(\mathcal{S}; \mathcal{T}) \geq \tau
$$

By supervising intermediate reasoning steps, process reward forces the trace to **actually engage with the input state** — to carry sufficient information about $\mathcal{S}$ through the reasoning chain rather than short-circuiting to an answer. It pushes the model back toward the IB frontier from below.

But this framing also reveals the fundamental limitation: **process reward can only enforce $I(\mathcal{S}; \mathcal{T}) \geq \tau$ along dimensions that the process reward signal itself can measure.** Every process reward has its own blind spots — its own null space of reasoning quality it cannot see.

### The Hierarchy of Process Reward Robustness

The robustness of a process reward signal depends on how **grounded** it is — how directly it can verify that information from $\mathcal{S}$ is faithfully preserved in $\mathcal{T}$:

1. **Formal verification** (proof assistants, code execution, unit tests): Each step is checked against an external formal system. Null space is minimal. Hardest to hack, but narrowly applicable.

2. **Execution-grounded signals** (intermediate computation results, simulation outputs): Steps are checked against physical or computational reality. Robust but domain-limited.

3. **Monte Carlo value estimates** (Math-Shepherd style): Step quality is estimated by sampling future rollouts and checking final outcomes. Statistically grounded in true reward, but noisy and expensive.

4. **Learned PRMs** (trained on human step-level labels): Captures human intuition about reasoning quality. Rich signal, but inherits human biases and is vulnerable to distribution shift.

5. **LLM-as-judge**: Cheapest, most scalable, most vulnerable to systematic blind spots. The judge and the student share similar inductive biases, creating **correlated failure modes** — they are likely to be blind to the same shortcuts.

**The further down this list, the larger the null space of the process reward signal, and the more room remains for collapse.** This is the recursive supervision problem: each layer of oversight introduces its own compression artifacts.

---

## 6. Phase Transitions and Scaling

This framework predicts **non-linear scaling behavior**.

For a given task with effective mapping complexity $k \approx I(\mathcal{S}; \mathcal{A})$, a model with capacity $c$ will exhibit:

- **$c \ll k$**: Model cannot represent the IB-optimal trace. Performance is near random or at a shallow heuristic baseline. RL provides no benefit because the policy space doesn't contain the solution.

- **$c \approx k$**: The model can just barely represent the mapping. RL begins to help, but training is fragile — the model is on the edge of its capacity, and reward hacking is most dangerous here because the model has just enough flexibility to find shortcuts but not enough to reliably find the true solution. **The IB frontier is barely reachable, and degenerate solutions below the frontier are close in parameter space.**

- **$c \gg k$**: The model has surplus capacity. The mapping is easy to learn, RL converges quickly, and the risk shifts from reward hacking to **overfitting** — memorizing training distribution patterns rather than generalizing.

The **most interesting and dangerous regime is $c \approx k$** — where the model is capable enough to learn something but not so capable that the right answer is the easy answer. This is exactly where most frontier RL training operates today.

---

## 7. Practical Implications

| Principle | Implication |
|-----------|------------|
| Estimate mapping complexity $I(\mathcal{S}; \mathcal{A})$ before choosing method | If low, use SFT. If high, RL has value. |
| Reward hacking risk ∝ gap between reward dimensionality and $I(\mathcal{S}; \mathcal{A})$ | For complex tasks, invest in richer reward signals or accept the risk. |
| Process reward = lower bound on $I(\mathcal{S}; \mathcal{T})$ | Use the most grounded signal available to tighten this bound. |
| The $c \approx k$ regime is where training is most fragile | Monitor for both collapse and instability when pushing capability boundaries. |
| Test-time compute and RL are complementary | RL optimizes the policy over $\mathcal{T}$; test-time compute expands the searchable $\mathcal{T}$ at inference. Both address mapping complexity through different mechanisms. |

---

## 8. Open Questions

1. **Can we measure $I(\mathcal{S}; \mathcal{A})$ empirically before training?** If so, we could predict which tasks benefit from RL, how large a model is needed, and where reward hacking will emerge.

2. **Can we design reward signals whose dimensionality matches the task complexity by construction?** Rather than patching hacking post hoc, can we build rewards with minimal null space for the dimensions that matter?

3. **What is the optimal curriculum in IB terms?** Training on problems of increasing $I(\mathcal{S}; \mathcal{A})$ — starting with low-complexity mappings and gradually increasing — may allow the model to build compositional reasoning primitives before tackling problems that require composing them.

4. **Is there a formal relationship between the IB frontier and emergent capabilities?** Phase transitions in model capability may correspond to the model's capacity crossing the $I(\mathcal{S}; \mathcal{A})$ threshold for a class of tasks — suddenly able to represent IB-optimal traces for problems it previously could not compress.

---

*Thanks to conversations with Claude for helping refine these ideas.*