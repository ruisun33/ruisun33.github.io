---
title: "Conservation of Knowledge: Why Every Good Reward Is Borrowed"
description: "Every reward signal that speeds up training is knowledge borrowed from somewhere. A conservation law for RL training signal — and the one place it breaks."
date: 2026-06-30
categories: [Thoughts]
tags: [reinforcement-learning, reward-shaping, RLVR, distillation, LLM-as-judge]
math: true
---

*A conservation law for RL training signal — and the one place it breaks.*

---

I've been reading a lot of reward papers lately: dense reward shaping, process reward models, teacher-distilled rewards, LLM-as-judge, hint injection, curriculum tricks. They come from different subfields and cite different ancestors, but after enough of them a single pattern starts to feel inevitable — and once you see it, you can't unsee it.

**Every reward signal that makes training faster is knowledge borrowed from somewhere. The signal doesn't create the knowledge; it moves it. And the ceiling of any method is the ceiling of the reservoir it borrowed from.**

I want to argue this is close to a conservation law, name where the knowledge actually comes from, and then point at the single case that escapes it — because that exception is the most important thing in the whole picture.

## Dense is not the axis. Information is.

The seductive story is: sparse reward is slow, dense reward is fast, so make rewards denser. Give the model a nudge on every step instead of one bit at the end, and it climbs the curve faster.

That story is *half* true, and the missing half is where all the confusion lives. Density is not what helps you. **Information is.** A denser reward helps only insofar as it carries *correct information the policy did not already have*.

The cleanest way to see this is a result that's now 25 years old. Potential-based reward shaping (Ng, Harada & Russell, 1999) proved that if you reshape a reward by adding a potential function $\Phi$, you can change *how fast* an agent learns without changing *what it eventually learns* — the optimal policy is invariant. Read that carefully. The shaping term is pure acceleration, and the acceleration you get is exactly the useful prior knowledge you baked into $\Phi$. If $\Phi$ encodes something true about the world, you converge faster. If it encodes nothing, you get nothing. If it encodes something false, you get led astray.

So the dense signal is a *channel*, and the value flowing through the channel is bounded by whatever sits at the source. This is really just the data-processing inequality wearing a reinforcement-learning costume: **no amount of clever processing downstream can manufacture information that wasn't upstream.** A reward model trained on human labels cannot know more than the humans knew. An LLM judge cannot rank answers more wisely than the model generating its judgments. The dense reward feels like free lift, but it's a loan against a reservoir you filled earlier.

## The four reservoirs

If every good reward is borrowed, the honest question is: *borrowed from whom?* I think there are exactly four sources, and they are not interchangeable. They differ in the one property that matters — how high their ceiling is.

**1. Humans.** RLHF reward models, human-written hints, human step-level annotations for process rewards. The knowledge is human preference and human correctness. Ceiling: us. Which means expensive, inconsistent, and — for anything at or beyond expert frontier — a hard cap. You cannot label your way past the smartest labeler.

**2. A stronger model.** Teacher distillation, LLM-as-judge, critique and refinement models. Here the reservoir is another network that already knows more. This is genuinely useful and cheap to scale, but its ceiling is stark: **you can approach the teacher, never exceed it.** Distillation is compression of a fixed thing. The student's asymptote is the teacher's frontier.

**3. The model's own latent knowledge, elicited.** Self-distillation, best-of-$N$, self-consistency, STaR/rejection-sampling loops, MCTS-derived process rewards. This is the one that *feels* like a free lunch — the model teaches itself, so where did the knowledge come from? It came from inside. These methods **sharpen** capability the base model already latently had; they concentrate probability mass onto good trajectories the model could already sometimes produce. That's elicitation, not creation. It's real, it's valuable, and it is strictly bounded by the base model's latent frontier. You can sample and sharpen a model into revealing what it knows; you cannot sample it into knowing something it doesn't.

**4. The environment.** A verifier: unit tests, a theorem prover, an exact-match math checker, a compiler, a physics simulator. And this one is different in kind — different enough that I want to give it its own section.

Notice the ordering. Human → stronger model → elicited self → environment is a ladder of *ceilings*, from lowest to highest. Most of the reward-engineering literature is a very sophisticated argument about how to move knowledge cheaply between the first three. Which is fine. But if your reservoir is bounded, no cleverness in the channel raises the bound.

## The exception that matters: verifiers

Reservoirs 1–3 are all *fixed stores of knowledge* — a snapshot of what some agent already knew. Distilling from them is redistribution. That's why the conservation law bites: you're moving a fixed quantity around, and the destination can't hold more than the source.

A verifier is not a store of knowledge. **It's an oracle over an effectively infinite space.** A theorem prover doesn't "know" the proofs; it can *check* any proof you hand it, including ones no human and no teacher has ever seen. A unit-test harness will validate a correct program you invented from scratch. The reservoir here isn't a snapshot — it's the structure of the problem itself, and that well doesn't run dry.

This is why RL with verifiable rewards (RLVR) can push *past* any teacher, and why that's where the frontier actually moved. When the reward is grounded in ground truth rather than distilled from a fixed knower, the policy can discover trajectories the reservoir never contained, because the reservoir never *contained* trajectories at all — it contained a *test*. Conservation of knowledge holds for the first three reservoirs. Verifiers are the loophole. If you want open-ended capability gain, you almost have to route through them.

The catch, of course, is coverage: verifiers exist for math, code, formal logic, games — domains with a crisp notion of correct. The moment you leave them, you slide back down to reservoirs 1–3 and the conservation law reasserts itself. "How do we build verifiers for open-ended domains" is, in this framing, *the* central problem of post-training, because it's literally the question of where the next unbounded reservoir comes from.

## What the law predicts

A framing is only worth writing down if it makes predictions you could be wrong about. This one does:

| The law says | The prediction | How you'd see it |
|---|---|---|
| Reservoir-bounded methods asymptote at their source | Distillation and LLM-judge pipelines plateau at the teacher/judge frontier, no matter how much compute you pour in | Diminishing returns that a bigger teacher fixes but more student steps don't |
| Elicitation ≠ creation | Self-distillation / best-of-$N$ improves pass@1 up to the base model's latent ceiling, then stalls | Gains that track base-model capability, and vanish on problems the base never solves even at high $N$ |
| Density without information is inert | A dense reward that's a smooth interpolation of a sparse one speeds convergence but doesn't raise the ceiling | Faster curves, same asymptote |
| Verifiers are unbounded | Only verifier-grounded RL reliably exceeds the strongest available teacher | Frontier capability gains concentrated in verifiable domains |

If you saw a pure distillation setup blow past its teacher with no verifier and no new data in the loop, this whole framing would be in trouble. I don't think you will.

## Why "conservation" and not just "rewards need good sources"

One honest caution, because I'd want a careful reader to trust the piece: stated loosely — "good rewards come from somewhere" — this is unfalsifiable mush. It only earns the word *conservation* when you tie it to information and to the create-vs-elicit distinction. The content of the claim is:

1. A reward signal accelerates learning **only** by transferring information the policy lacks (data-processing inequality).
2. For reservoirs 1–3, that information is a **fixed quantity** — a snapshot of an existing knower — so the destination is bounded by the source.
3. Verifiers are the **exception** because they're oracles, not snapshots, and their reservoir is unbounded.

That's the whole law. Dense reward, teacher reward, LLM-judge, hints, process rewards — every technique in the pile I've been reading is a different engineering answer to "which reservoir, and how cheaply do I tap it." Seen that way, the field stops looking like a grab-bag of tricks and starts looking like a resource-allocation problem with one scarce, precious input: **grounded, unbounded signal.**

## Practical upshots

- **Before adding a dense reward, ask what it *knows* that the sparse one doesn't.** If the answer is "nothing, it's just smoother," you'll get a faster curve to the same place — sometimes worth it, but don't confuse it with a higher ceiling.
- **Match ambition to reservoir.** Trying to exceed frontier capability with LLM-judge rewards is a category error; you've capped yourself at the judge. Use judges to cheaply *approach* a known ceiling, not to break one.
- **Self-distillation is an elicitation budget, not a capability budget.** It's the right tool when you believe the model already knows more than it reliably shows. It's the wrong tool when you need it to learn something genuinely new.
- **Spend your hardest engineering on verifiers.** The highest-leverage move in the whole taxonomy is turning a fuzzy domain into a checkable one. That's not reward *tuning* — it's reservoir *creation*, and it's the only thing that lifts the ceiling.

The reward is never the source of the intelligence. It's the pipe. The interesting question was always what you connected the other end to.

---

*Thanks to conversations with Claude for helping sharpen these ideas.*
