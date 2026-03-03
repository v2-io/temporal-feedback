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
| [Appendix F](Appendix-F-Multi-Agent.md) | Multi-Agent Coupling and Strategic Interaction | Formulation + Discussion | Draft |

## Reading Guide

**Core theorem path** (minimal chain for the main formal results): TF-01 (scope) → TF-03 (model) → TF-05 (mismatch) → TF-06 (gain) → TF-11 (tempo & persistence) → Appendix A (Lyapunov stability). This chain contains the main formal backbone and can be read in ~45 minutes. [Appendix E](Appendix-E-TFT-Core.md) presents this chain as a self-contained condensed reference. TF-00 defines the five-phase loop vocabulary — Prolepsis, Aisthesis, Aporia, Epistrophe, Praxis — used throughout the documents.

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

**Multi-agent and distributed model theory.** [Appendix F](Appendix-F-Multi-Agent.md) now provides the initial multi-agent extension: communication gain (extending TF-06's uncertainty ratio to social channels), distributed tempo, cooperative/adversarial disturbance decomposition, and topology-dependent analysis. This is exploratory — it lays out the coupling framework and identifies where game theory and decision theory enter, but does not yet contain formal propositions or empirical validation (it does include explicit falsification predictions targeting the three main hypotheses). The framework supports collaborative peers, ensembles, and hierarchical organizations; convergence conditions and $N$-agent equilibrium analysis remain open, and a Bayesian transitive-trust mechanism with risk-asymmetric quantile adjustment is proposed but untested.

**Game theory of communication.** TF-08 introduces adversarial query dynamics and Appendix F extends these to $N$-agent networks. The game-theoretic question — *when* is honest communication incentive-compatible? — is one TFT makes formally precise (through the alignment uncertainty $U_{\text{align},ji}$ in the communication gain) but does not answer. The literature on cheap talk, signaling games, and mechanism design addresses this. Integrating even basic results (e.g., conditions under which communication is credible) would strengthen the adversarial treatment and connect TFT to a mature formal discipline.

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

## TODO

- **Decompose TF-02, which is overloaded.** TF-02 is the causal structure axiom — "time has a direction." But it currently also houses material that belongs where it first becomes load-bearing:

  - **TF-02 should keep**: The axiom (temporal arrow), causality as temporal ordering, Pearl's three levels of epistemic access (direct consequence of the axiom), and the coupling strength spectrum.
  - **CIY machinery → move to just before or into TF-08** (exploration-exploitation, where CIY first enters an equation): canonical CIY definition, CIY proxy, admissibility regimes (A/B/C), identifiability gate. These form a cohesive package — "how to quantify and use causal information from actions" — and belong where the theory starts using them, not where their prerequisite axiom lives.
  - **Agent identity / clone problem → separate as its own discussion-grade TF or appendix.** Not used by any downstream formal result; currently TF-02 itself says "skip if on theorem track."

  Guiding principle: each TF-NN should name one principle piece of the theory, following the TST convention where T-NN is synonymous with the theorem/formulation it names.

- **Restructure TF-03: move recursive update to TF-02, defer $S$ to where it's load-bearing.** TF-03 currently presents the recursive update $M_t = f(M_{t-1}, o_t, a_{t-1})$ as "derived" from model sufficiency ($S \approx 1$). This is misleading — the recursive form follows from the arrow of time (TF-02) plus the definition of model state as the agent's complete internal state, not from sufficiency. Sufficiency tells you how *good* the recursion is, not whether you *can* recurse. As currently written, TF-03 appears to derive "what it takes to be adaptive" when it's really defining the scope of analysis.

  **Proposed restructuring:**

  - **Move the recursive update to TF-02** (causal structure), presented as a consequence of the temporal axiom. At any event, the agent's available information is exactly $(M_{\tau^-}, e_\tau)$ — its prior complete state plus the new event. The arrow of time forbids dependence on future events; partial observability (TF-01) forbids direct access to $\Omega$; completeness of $M$ makes dependence on earlier states redundant. Therefore $M_{\tau^+} = f(M_{\tau^-}, e_\tau)$ is the unique maximally general causal-respecting update form. This should be presented in event-driven generality from the start — covering asynchronous multi-channel events at variable times with continuous timestamps, not just the serial single-channel special case. Between events, the model may evolve autonomously: $dM/d\tau = g(M_\tau)$ (prediction, decay, internal simulation), also constrained to depend only on current state since no new external information arrives. The serial form $M_t = f(M_{t-1}, o_t, a_{t-1})$ is then the uniform-rate single-channel special case. Ideally this includes a clean proof that this is the *unique* proper update form given the axiom and scope — not merely one option but the only one consistent with directed time, partial observability, and state completeness.

  - **TF-03 retains**: the model as compression ($\phi$, $\mathcal{M}$), the information bottleneck, the model space table, the "formulation not axiom" discussion. These are about *what the model is and how it should compress* — representation and trade-offs, independent of the update dynamics.

  - **Defer $S(M_t)$, $S_\Pi(M_t)$, and $\mathcal{F}(\mathcal{M})$ to where they're first load-bearing.** The sufficiency score is not needed for the information bottleneck (which uses raw mutual information, not the normalized ratio). $S$ is primarily machinery for TF-10's Proposition 10.1 (structural adaptation necessity) and secondarily for TF-05's bridge to mismatch inevitability. Introducing $S$ in TF-03 — six documents before it's formally needed — creates the false impression that it's a prerequisite for the IB and recursive update. Consider: introduce $S$ and $\mathcal{F}$ just before TF-10, or as a standalone brief TF between TF-05 and TF-10, so each concept arrives where the reader needs it.

  - **Open question for the recursive update proof**: TF-03 currently provides no bounds on per-step information loss when $S < 1$ — how much the recursive approximation degrades compared to (infeasible) batch reprocessing, and whether this error compounds, stabilizes, or self-corrects through the feedback loop. This gap should be addressed, possibly as part of the $S$ discussion wherever it lands.

## Planned Companion Documents

1. **LLM Training Strategy** (not yet started): A research, experimentation, and development plan for LLM training grounded in TFT plus additional references. Concepts from the v7 dialogue — agentic anima, logozoetic intelligence, continuous orientation, developmental psychology framing (Erikson) — reserved for this document as a "first practical use case" of TFT.
