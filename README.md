# Temporal Feedback Theory (TFT) — Working Draft

A first-principles formal theory of adaptive feedback: how agents persist and adapt under uncertainty through model maintenance, mismatch detection, and structured updating.

This theory proposes a formal structure underlying Boyd's OODA loop, Kalman filtering, reinforcement learning, PID control, Bayesian inference, active inference, PDCA/Lean, biological immune systems, and organizational learning — analyzing each as an *instance* of a common adaptive pattern rather than independent frameworks. The degree to which this structural analogy constitutes genuine unification (vs. a useful modeling lens) is itself an open question addressed throughout.

TFT provides a **shared formal vocabulary** across adaptive-system disciplines. It enables a control theorist, an RL researcher, an organizational strategist, and a military planner to describe the same underlying adaptive mechanics using the same mathematical objects — mismatch, gain, tempo, reserve — with domain-specific instantiations that are formally comparable. The theory's primary utility today is as a *diagnostic and design framework*: it identifies failure modes (under-updating, over-updating, parametric vs. structural inadequacy, tempo insufficiency, reserve depletion) using quantities that can be estimated from system traces (see [Appendix B](Appendix-B-Operationalization.md)).

The document structure is modeled on [Temporal Software Theory](../temporal-software-theory/), building piece by piece from axioms through definitions to derived results, with each claim's epistemic status explicitly marked.

## Table of Contents

| # | Name | Type | Status |
|---|------|------|--------|
| [TF-00](TF-00.md) | Notation and Conventions | Reference | Draft |
| [TF-01](TF-01.md) | Scope — Adaptive Agents Under Uncertainty | Scope Definition | Draft |
| [TF-02](TF-02.md) | Causal Structure | Axiom | Draft |
| [TF-03](TF-03.md) | The Model | Formulation | Draft |
| [TF-04](TF-04.md) | Event-Driven Dynamics | Formulation | Draft — placement & formulation under review |
| [TF-05](TF-05.md) | The Mismatch Signal | Derived | Draft |
| [TF-06](TF-06.md) | The Update Gain | Derived + Empirical Claim | Draft |
| [TF-07](TF-07.md) | Action Selection | Derived + Discussion | Draft |
| [TF-08](TF-08.md) | The Exploration-Exploitation Balance | Hypothesis + Discussion | Draft |
| [TF-09](TF-09.md) | The Cost of Deliberation | Derived (conditional on local Assumption 9.0) | Draft |
| [TF-10](TF-10.md) | Structural Adaptation | Derived + Empirical | Draft |
| [TF-11](TF-11.md) | Temporal Nesting and Adaptive Tempo | Derived + Hypothesis | Draft |
| [Appendix A](Appendix-A-Lyapunov.md) | Lyapunov Stability Analysis | Derived (from sector-condition assumptions) | Draft |
| [Appendix B](Appendix-B-Operationalization.md) | Operationalization: Estimation Procedures | Procedures | Draft |
| [Appendix C](Appendix-C-Kalman-Example.md) | Worked Example — 1D Active Tracking (Kalman) | Worked Example | Draft |
| [Appendix D](Appendix-D-RL-Example.md) | Worked Example — Nonstationary Bandit (RL) | Worked Example | Draft |
| [Appendix E](Appendix-E-TFT-Core.md) | TFT Core: Minimal Formal Path | Reference | Draft |

## Reading Guide

**Core theorem path** (minimal chain for the main formal results): TF-01 (scope) → TF-03 (model) → TF-05 (mismatch) → TF-06 (gain) → TF-11 (tempo & persistence) → Appendix A (Lyapunov stability). This chain contains the main formal backbone and can be read in ~45 minutes. [Appendix E](Appendix-E-TFT-Core.md) presents this chain as a self-contained condensed reference.

**Supporting material**: TF-00 (notation — consult as needed), TF-02 (causal structure — foundational but conceptual), TF-04 (event-driven dynamics — generalizes discrete-time notation), TF-07 (action selection), TF-08 (exploration-exploitation), TF-09 (deliberation cost), TF-10 (structural adaptation). These enrich the theory with conceptual depth and domain connections but are not required for the main formal results.

**Operationalization**: Appendix B (estimation procedures), Appendix C (Kalman worked example — exact mapping), and Appendix D (RL worked example — approximate mapping). Start here if you want to *apply* TFT to a specific system.

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

The theory builds from a scope definition, one axiom, and one formulation:

1. **TF-01** defines the scope: agents coupled to environments through observation and action under residual uncertainty.
2. **TF-02** establishes the **causal structure**: the irreversible temporal ordering of events is constitutive of the feedback loop. Causality here is grounded in temporal ordering (the most primitive notion) rather than statistical influence — the causal structure holds even when the agent's actions have minimal or nominal effect on the environment. This axiom grounds Pearl's causal hierarchy (associational → interventional → counterfactual) as three levels of epistemic access available within the loop, and establishes that an agent's interaction history is a singular, non-forkable causal trajectory.
3. **TF-03** formulates the analytical framework: any persisting agent is analyzed as maintaining a **model** — a compression of its interaction history that functions as a sufficient statistic for prediction and action. This is a representational definition (not a psychological claim) that becomes productive when the formal apparatus (sufficiency, information bottleneck, model class fitness) can be meaningfully applied.

From these, the remaining results follow:

- The **mismatch signal** (TF-05) exists as a consequence of having a predictive model in an uncertain world. TF-02 adds that this signal, when conditioned on the agent's action, carries *interventional* (causal) information.
- The **update gain** (TF-06) balances model uncertainty against observation uncertainty, with the Kalman gain, Bayesian posterior weight, and RL learning rate as special cases of a common form.
- **Action selection** (TF-07) is a function of the model, with a spectrum from implicit action (high *action fluency* — the degree to which effective action flows from the model without deliberation) to explicit deliberation. Selective pressure favors internalizing frequently-needed patterns, but deliberation remains essential for novel situations, large action spaces, and high-stakes decisions. Actions generate information (CIY, TF-02), and **query actions** — accessing another agent's pre-compressed model — can yield orders-of-magnitude higher CIY than direct environment probing, which is why rational agents preferentially consult reliable external models when available. CIY-dependent policy terms assume interventional identifiability conditions (Regime A directly, Regime B with explicit causal assumptions).
- **The cost of deliberation** (TF-09) formalizes the trade-off between action quality and timeliness: deliberation is justified iff the gain in update quality exceeds the mismatch accumulated during the deliberation period. The derivation is conditional on a local pause-window drift assumption (Assumption 9.0), with TF-11 providing a compatible global dynamics model.
- **Structural adaptation** (TF-10) — changing the model class, not just its parameters — becomes necessary when the model class itself is inadequate (Proposition 10.1). Parametric and structural adaptation are the most commonly analyzed ends of a *continuum* of adaptive timescales; real systems may operate at multiple intermediate levels simultaneously. Mechanisms of structural change range from expansion and grafting to full decomposition-and-recombination. The opposite failure mode, structural *overfitting*, is also addressed. The rational conservatism toward structural change is a derived consequence of TF-09's deliberation trade-off applied at a slower timescale: structural adaptation is deliberation with a massive $\Delta\tau$.
- **Temporal nesting** (TF-11) and the concept of **adaptive tempo** ($\mathcal{T} = \sum_k \nu^{(k)} \cdot \eta^{(k)*}$) yield the persistence threshold (Proposition 11.1) and formalize adversarial advantage. In the linear coupled model, adversarial mismatch scaling is a first-order heuristic; **Appendix A** provides the stronger reserve-threshold instability criterion via Lyapunov analysis, proving persistence and adversarial destabilization under general nonlinear correction dynamics (not just the linear hypothesis), and introducing the concept of *adaptive reserve* — an agent's shock tolerance.

## What's Probably Missing

**The axiomatic structure may need refinement.** The theory currently has one scope definition (TF-01), one axiom (TF-02, causal structure), and one formulation (TF-03, the model). Whether a more fundamental axiom exists — analogous to TST's T-01, which is a clean tautology about time optimality — is an open question. One possibility: TF-02's temporal arrow is already the right foundational axiom, and TF-01's agent-environment boundary is correctly identified as a scope definition rather than an axiom (since the boundary is a modeling choice, not a physical necessity). Another: there is a deeper tautology from which both the agent-environment distinction and the temporal arrow follow. This structural question remains open but has narrowed with TF-03's reclassification from axiom to formulation.

**Causality is addressed and partially integrated.** TF-02 grounds causality in temporal ordering and establishes Pearl's hierarchy as levels of epistemic access. TF-07 now references TF-02's foundation rather than re-deriving the hierarchy. CIY is now split into a canonical interventional quantity (non-negative by construction) and an observational proxy (diagnostic, assumption-sensitive); its connection to TF-05's mismatch signal (the mismatch conditioned on action carries interventional information) is noted but not yet formally threaded through as a theorem.

**Compression quality as a standalone result.** The nature of the compression (what the model preserves vs. discards) determines the agent's capabilities and blind spots. This connects to rate-distortion theory and representation learning. Currently embedded in TF-03's information bottleneck formulation; may warrant its own TF if it generates distinct derived results.

**Scope boundaries document.** The v7 source material had a section on where the OODA pattern breaks down (pure math, self-organizing systems, aesthetics). This should be carried forward, possibly as an appendix or a specific TF on boundaries.

**A formal domain instantiation appendix.** [Appendix B](Appendix-B-Operationalization.md) provides the operationalization framework, [Appendix C](Appendix-C-Kalman-Example.md) gives an exact end-to-end Kalman instantiation, and [Appendix D](Appendix-D-RL-Example.md) gives an approximate RL instantiation. A fuller consolidated mapping across all domains (PID, Bayesian, Boyd, PDCA, immune, organizational) is still pending.

## What's Potentially Missing

**A unified mathematical expression.** The claim that $\eta^* = U_M/(U_M + U_o)$ is the universal form of optimal updating is validated exactly for Kalman and Bayesian cases, approximately for RL, and loosely for PID. Whether a single equation can genuinely unify these — or whether the structural analogy is the real content and formal unification is a mirage — is one of the deeper open mathematical question incidental to this work. Confidence in genuine partial unification: ~60-65%. Full unification under one equation: ~30%.

**Treatment of continuous interiority / temporal continuity.** The event-driven formulation (TF-04) allows for model evolution *between* events ("between events, the model state may evolve autonomously"). This sentence is doing enormous work that isn't unpacked. For LLM architecture specifically, the question of whether the model should be *continuously* active (persistent orientation as default mode, with message-emission as deliberate action) vs. event-triggered (the current chat paradigm) is a critical design question with implications for consciousness, agency, and temporal awareness. See the v7 dialogue and the planned LLM strategy document (#2).

**Multi-agent and distributed model theory.** The current theory treats a single agent. Organizational learning, swarm intelligence, and multi-agent systems involve distributed models with coordination costs. The adversarial two-agent case in TF-11 is a start but doesn't address cooperative distributed adaptation.

**Game theory of communication.** TF-08 introduces adversarial query dynamics (deception as adversarial disturbance injection, active OODA loop interference via the information channel) but explicitly notes that the game-theoretic question — *when* is honest communication incentive-compatible? — is one TFT makes formally precise but does not answer. The literature on cheap talk, signaling games, and mechanism design addresses this. Integrating even basic results (e.g., conditions under which communication is credible) would strengthen the adversarial treatment and connect TFT to a mature formal discipline.

**The relationship to thermodynamics / entropy.** Boyd's original theory appealed to the second law. The Free Energy Principle claims thermodynamic grounding. Our theory currently grounds in information theory (sufficient statistics, information bottleneck) rather than thermodynamics. Whether there's a genuine connection (beyond analogy) to thermodynamic entropy and dissipative structures is unresolved. We are deliberately *not* assuming this connection, but it or its absence in these open systems may emerge.

## Falsification Predictions

If TFT's core claims are wrong, specific observable failures should occur. These predictions are testable in well-instrumented systems (Kalman tracking, RL benchmarks, organizational case studies):

1. **Gain structure.** TF-06 predicts that the optimal update gain has the structural form $\eta^* = U_M / (U_M + U_o)$. **Falsified if**: a well-studied adaptive system's optimal gain is shown to depend on a fundamentally different functional relationship — not merely a matrix or per-parameter generalization of the ratio, but a qualitatively different structure (e.g., optimal gain depends on mismatch *magnitude* rather than uncertainty ratio). Note: the claim is about the *structural form*, not the scalar expression.

2. **Persistence threshold.** TF-11 and Appendix A predict that an agent with adequate adaptive tempo ($\mathcal{T} > \rho / \|\delta_{\text{critical}}\|$) and a correction function satisfying the sector condition should maintain bounded mismatch. **Falsified if**: an agent demonstrably operating within its model class capacity ($\|\delta\| < R$) with tempo above the threshold still shows unbounded mismatch growth under bounded disturbance. This would indicate that the sector condition is insufficient for stability, or that a qualitatively different correction pathology exists.

3. **Speed-quality substitutability.** TF-11 predicts that $\mathcal{T} = \nu \cdot \eta^*$: doubling update quality has the same effect on steady-state mismatch as doubling event rate. **Falsified if**: in a controlled experiment, doubling $\eta^*$ (better model calibration) produces a systematically different effect on $\|\delta\|_{ss}$ than doubling $\nu$ (faster observation), after controlling for other factors. Non-substitutability would indicate the multiplicative structure is wrong.

4. **Adversarial tempo advantage (Corollary 11.2).** TF-11's Corollary 11.2 predicts that steady-state mismatch ratio scales with the *square* of the tempo ratio: $\|\delta_B\|_{ss}/\|\delta_A\|_{ss} \approx (\gamma_A/\gamma_B)(\mathcal{T}_A/\mathcal{T}_B)^2$. Appendix A predicts that destabilization occurs when $\gamma_A \cdot \mathcal{T}_A > \Delta\rho^*_B$. **Falsified if**: in adversarial settings, tempo advantage does not compound — i.e., doubling one agent's tempo relative to the other produces only a linear (not superlinear) increase in the slower agent's mismatch. This would indicate the coupling model is too simple.

5. **Structural adaptation trigger.** TF-10 predicts that persistent structured residuals (autocorrelated, patterned mismatch) after parametric convergence indicate model class inadequacy, and that switching model classes should reduce the mismatch floor. **Falsified if**: systematic residual patterns persist despite model class changes that provably increase $\mathcal{F}(\mathcal{M})$. This would indicate that structured residuals can arise from sources other than model class limitations (e.g., systematic observation bias not captured by TF-01's noise model).

These are not exhaustive, but they target the theory's most load-bearing claims. A failure in (1) or (2) would undermine the core framework; failures in (3)–(5) would indicate specific hypotheses that need refinement while leaving the broader structure intact.

## Source Material

- `ooda-loop-universal-pattern-v7.md` — The cross-domain analysis and dialogue from which this theory was distilled
- `../temporal-software-theory/` — The structural model for how this theory is organized (axiom → definition → derived → hypothesis)

## Planned Companion Documents

1. **LLM Training Strategy** (not yet started): A research, experimentation, and development plan for LLM training grounded in TFT plus additional references. Concepts from the v7 dialogue — agentic anima, logozoetic intelligence, continuous orientation, developmental psychology framing (Erikson) — reserved for this document as a "first practical use case" of TFT.
