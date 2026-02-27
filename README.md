# Temporal Feedback Theory (TFT) — Working Draft

A first-principles formal theory of adaptive feedback: how agents persist and adapt under uncertainty through model maintenance, mismatch detection, and structured updating.

This theory distills the universal pattern underlying Boyd's OODA loop, Kalman filtering, reinforcement learning, PID control, Bayesian inference, active inference, PDCA/Lean, biological immune systems, and organizational learning — treating each as an *instance* of a common formal structure rather than independent frameworks.

The document structure is modeled on [Temporal Software Theory](../temporal-software-theory/), building piece by piece from axioms through definitions to derived results, with each claim's epistemic status explicitly marked.

## Table of Contents

| # | Name | Type | Status |
|---|------|------|--------|
| [TF-00](TF-00.md) | Notation and Conventions | Reference | Draft |
| [TF-01](TF-01.md) | Scope — Adaptive Agents Under Uncertainty | Scope Definition | Draft |
| [TF-01b](TF-01b.md) | Causal Structure | Axiom | Draft — may become TF-02, pushing others down |
| [TF-02](TF-02.md) | The Model | Axiom | Draft |
| [TF-03](TF-03.md) | Event-Driven Dynamics | Formulation | Draft — placement & formulation under review |
| [TF-04](TF-04.md) | The Mismatch Signal | Derived | Draft |
| [TF-05](TF-05.md) | The Update Gain | Derived + Empirical Claim | Draft |
| [TF-06](TF-06.md) | Action Selection | Derived + Discussion | Draft |
| [TF-07](TF-07.md) | Structural Adaptation | Derived + Empirical | Draft |
| [TF-08](TF-08.md) | Temporal Nesting and Adaptive Tempo | Derived + Hypothesis | Draft |

## Epistemic Status Legend

- **Axiom**: Deliberately tautological or information-theoretically necessary. Not empirically falsifiable within scope.
- **Scope Definition**: Defines where the theory applies. Not a claim about reality but a boundary condition.
- **Formulation**: A representational choice that is more general than alternatives. Not axiomatic but adopted for good reasons.
- **Definition**: Names a mathematical object or quantity. No truth claim.
- **Derived**: Follows logically from prior axioms, definitions, and results.
- **Empirical Claim**: Validated exactly in some domains, approximately in others. Needs broader validation.
- **Hypothesis**: A first-order approximation or conjectured relationship. Qualitatively robust but quantitatively unvalidated.
- **Discussion**: Conceptual consequences and properties that follow qualitatively but are not formally proven here.

## The Core Structure

The theory builds from a scope definition and two axioms:

1. **TF-01** defines the scope: agents coupled to environments through observation and action under residual uncertainty.
2. **TF-01b** establishes the **causal structure**: the irreversible temporal ordering of events is constitutive of the feedback loop. Causality here is grounded in temporal ordering (the most primitive notion) rather than statistical influence — the causal structure holds even when the agent's actions have minimal or nominal effect on the environment. This axiom grounds Pearl's causal hierarchy (associational → interventional → counterfactual) as three levels of epistemic access available within the loop, and establishes that an agent's interaction history is a singular, non-forkable causal trajectory.
3. **TF-02** establishes that any persisting agent necessarily maintains a **model** — a compression of its interaction history that functions as a sufficient statistic for prediction and action.

From these, the remaining results follow:

- The **mismatch signal** (TF-04) exists as a consequence of having a predictive model in an uncertain world. TF-01b adds that this signal, when conditioned on the agent's action, carries *interventional* (causal) information.
- The **update gain** (TF-05) balances model uncertainty against observation uncertainty, with the Kalman gain, Bayesian posterior weight, and RL learning rate as special cases of a common form.
- **Action selection** (TF-06) is a function of the model, with implicit/habitual action as the norm and deliberation as the fallback. The exploration-exploitation trade-off connects to maximizing causal information yield (TF-01b).
- **Structural adaptation** (TF-07) — changing the model class, not just its parameters — is necessary when the model class itself is inadequate.
- **Temporal nesting** (TF-08) and the concept of **adaptive tempo** ($\mathcal{T} = \sum_k \nu^{(k)} \cdot \eta^{(k)*}$) yield the persistence threshold and formalize adversarial advantage.

## What's Probably Missing

**TF-01 may not be the right starting axiom.** The current TF-01 is a scope definition + primitive objects. If a more fundamental axiom exists (analogous to TST's T-01, which is a clean tautology about time optimality), it hasn't been identified yet. TF-01b (causal structure) is a candidate for the true foundational axiom — the arrow of time may be more primitive than the agent-environment distinction. This structural question remains open.

**Causality is addressed and partially integrated.** TF-01b grounds causality in temporal ordering and establishes Pearl's hierarchy as levels of epistemic access. TF-06 now references TF-01b's foundation rather than re-deriving the hierarchy. The causal information yield (CIY) is defined in TF-01b as a genuine information-theoretic quantity; its connection to TF-04's mismatch signal (the mismatch conditioned on action carries interventional information) is noted but not yet formally threaded through as a theorem.

**Compression quality as a standalone result.** The nature of the compression (what the model preserves vs. discards) determines the agent's capabilities and blind spots. This connects to rate-distortion theory and representation learning. Currently embedded in TF-02's information bottleneck formulation; may warrant its own TF if it generates distinct derived results.

**Scope boundaries document.** The v7 source material had a section on where the OODA pattern breaks down (pure math, self-organizing systems, aesthetics). This should be carried forward, possibly as an appendix or a specific TF on boundaries.

**A formal domain instantiation appendix.** The individual TFs contain domain mapping tables. A consolidated appendix showing the complete mapping for each domain (Kalman, RL, PID, Bayesian, Boyd, PDCA, immune, organizational) would be valuable.

## What's Potentially Missing

**A unified mathematical expression.** The claim that $\eta^* = U_M/(U_M + U_o)$ is the universal form of optimal updating is validated exactly for Kalman and Bayesian cases, approximately for RL, and loosely for PID. Whether a single equation can genuinely unify these — or whether the structural analogy is the real content and formal unification is a mirage — is the deepest open mathematical question. Confidence in genuine partial unification: ~60-65%. Full unification under one equation: ~30%.

**Treatment of continuous interiority / temporal continuity.** The event-driven formulation (TF-03) allows for model evolution *between* events ("between events, the model state may evolve autonomously"). This sentence is doing enormous work that isn't unpacked. For LLM architecture specifically, the question of whether the model should be *continuously* active (persistent orientation as default mode, with message-emission as deliberate action) vs. event-triggered (the current chat paradigm) is a critical design question with implications for consciousness, agency, and temporal awareness. See the v7 dialogue and the planned LLM strategy document (#2).

**Multi-agent and distributed model theory.** The current theory treats a single agent. Organizational learning, swarm intelligence, and multi-agent systems involve distributed models with coordination costs. The adversarial two-agent case in TF-08 is a start but doesn't address cooperative distributed adaptation.

**The relationship to thermodynamics / entropy.** Boyd's original theory appealed to the second law. The Free Energy Principle claims thermodynamic grounding. Our theory currently grounds in information theory (sufficient statistics, information bottleneck) rather than thermodynamics. Whether there's a genuine connection (beyond analogy) to thermodynamic entropy and dissipative structures is unresolved. We are deliberately *not* assuming this connection, but it may emerge.

## Source Material

- `ooda-loop-universal-pattern-v7.md` — The cross-domain analysis and dialogue from which this theory was distilled
- `../temporal-software-theory/` — The structural model for how this theory is organized (axiom → definition → derived → hypothesis)

## Planned Companion Documents

1. **LLM Training Strategy** (not yet started): A research, experimentation, and development plan for LLM training grounded in TFT plus additional references. Concepts from the v7 dialogue — agentic anima, logozoetic intelligence, continuous orientation, developmental psychology framing (Erikson) — reserved for this document as a "first practical use case" of TFT.
