# Conversation Conclusions & Design Decisions
## (Working reference for continuity across sessions)

## Project Overview

We are distilling the universal feedback loop pattern identified in
`ooda-loop-universal-pattern-v7.md` into a first-principles formal theory,
modeled after the structure of Temporal Software Theory (TST, ~/src/temporal-software-theory/).

Two deliverables planned:
1. **This document set**: The universal pattern as a formal theory (working name: "Temporal Feedback Theory" or TFT)
2. **A separate document** (future): LLM training strategy/research plan grounded in #1 plus other references Joseph will provide

## Key Design Decisions

### Framing
- Boyd's OODA is an *instance* of the universal pattern, not the organizing frame
- Same for Kalman, RL, PID, PDCA, active inference, etc. — all instances
- Strip domain-specific baggage; present the pattern on its own terms
- Boyd himself treated his work as principled truths universally available, not personal contribution
- Concepts from the v7 dialogue (agentic anima, logozoetic intelligence, continuous orientation, developmental framing) reserved for document #2 but kept in mind as "first practical use case"

### Audience & Quality
- First: ourselves as working reference
- Quality bar: truthful and rigorous enough to publish in any context
- Should be able to generate different granularity presentations from the core

### Structure (modeled on TST)
- Build piece by piece: axioms → definitions → derived theorems
- Each claim marked with epistemic status: axiom, definition, derived, empirical, hypothesis
- Open questions noted explicitly
- NOT a monolithic essay arguing importance — a formal sequence where implications emerge from the math
- Three levels:
  1. **Level 1 — The pattern itself**: formal structure, components, relationships
  2. **Level 2 — Properties and derived claims**: things like model primacy, destructive renewal (derived, not axiomatic)
  3. **Level 3 — Instantiations**: each domain as a special case with explicit mappings

### Core Correction on Framing
- The "core claim" is NOT "model quality dominates outcomes" (that's a derived finding)
- The core is: "Here is the structure that characterizes adaptive agents in open systems"
- Something like: "the fitness of an agent can be modeled with the following fundamental properties..."
- The dominance of model quality then FOLLOWS from the structure

## Mathematical Framework — Agreed Foundations

### Notation
We define our own clean symbols, not importing any single domain's conventions:
- H_t — interaction history
- M_t — model (the central object), drawn from model space M
- o_t — observation
- a_t — action
- δ_t — mismatch signal
- η_t — update gain
- φ — compression function (history → model)
- f — recursive update function
- π_M — action selection given model

### Core Mathematical Objects

**Interaction History** (minimal, assumption-free starting point):
  H_t = (o_1, a_1, o_2, a_2, ..., o_t)

**Model as Compression / Sufficient Statistic**:
  M_t = φ(H_t)

  Good model = sufficient statistic: preserves information relevant for
  prediction and action selection, discards the rest.

  Information bottleneck formulation:
  - Minimize I(M_t; H_t) [compression]
  - Maximize I(M_t; o_{t+1}, o_{t+2}, ... | a_t, a_{t+1}, ...) [predictive power]

**Model Space & Structural Flexibility**:
  M_t ∈ M where M is the space of possible models.
  The agent's epistemic state may be:
  - A single model (point estimate)
  - A distribution over models P(M_t) ∈ Δ(M) (Bayesian)
  - An ensemble {M_t^(1), ..., M_t^(k)} (particle filter)
  - A mixture with weights (mixture of experts)

  This decouples iterative update dynamics from structural model commitments.

  Destructive renewal = switching M itself (meta-level):
  Agent maintains (M_t, M_t) where M_t changes on slower timescale.

**Recursive Update**:
  M_t = f(M_{t-1}, o_t, a_{t-1})

  Possible because M_t is a sufficient statistic — don't need full history.

**Mismatch Signal**:
  δ_t = o_t - ô_t(M_{t-1}, a_{t-1})

  (Observed minus predicted.) This IS the Kalman innovation, TD error,
  prediction error, free energy gradient — all at once.

**Update Rule** (general additive form):
  M_t = M_{t-1} + η(M_{t-1}, o_t) · g(δ_t)

**Optimal Gain** (the potential unification):
  η* ∝ (model uncertainty) / (model uncertainty + observation uncertainty)

  Kalman: K_t = P_t H^T (H P_t H^T + R)^{-1} — exactly this ratio
  Bayesian: posterior weight governed by same trade-off
  RL: learning rate should be high when value estimates uncertain (UCB, Bayesian RL)
  PID: tuning constants determine trust in error signal vs current state

  Confidence in genuine universality of this form: ~55-65%
  Works cleanly for Kalman/Bayesian. Approximately for RL. Loosely for PID.
  For non-parametric models, "model uncertainty" not well-defined as single quantity.

### Event-Driven Temporal Formulation

The discrete time step t is a convenience. Real systems have multi-rate
observations and actions. The theory should be stated in terms of
**information arrival events**:

  E = {(e_1, τ_1), (e_2, τ_2), ...}

where each event is an observation from channel o^(k) or action completion a^(j),
with timestamp τ_i.

  M_{τ+} = f(M_{τ-}, e_τ)

Key insight: "loop rate" is not a property of the agent as a whole but of
each observation-action channel. An organism has fast loops (reflexes, ms),
medium loops (perception-action, 100s of ms), slow loops (deliberation, s to hrs).
These are the SAME model being updated by events at different rates.

Avoids aliasing entirely — responding to events, not sampling at fixed rate.

Discrete-time form is the special case where events arrive uniformly on
a single channel.

### Derived: Speed Advantage ("Inside the Opponent's OODA Loop")

Effective adaptation rate — rate at which mismatch decreases:

  d|δ|/dt = -ν · η* · |δ| + ρ(t)

where ν = event/update rate, η* = update quality, ρ = environment change rate.

Steady state:
  |δ|_ss = ρ / (ν · η*)

Derived results:
1. "All else equal, faster is better" — increasing ν decreases |δ|_ss
2. "Model quality substitutes for speed" — doubling η* = doubling ν
3. Adversarial case (two coupled agents):
   ρ_B ∝ ν_A · η_A* (A's actions are B's environment changes)
   |δ_B|_ss = (ν_A · η_A*) / (ν_B · η_B*)
   When ratio > 1, B's model falls behind — "acting on stale world"
   This IS "getting inside the opponent's loop," derived not asserted
4. Critical threshold: when ν_A · η_A* >> ν_B · η_B*, B's mismatch
   grows without bound — Boyd's "effects spiral"
5. Non-adversarial: need ν · η* > ρ to keep mismatch bounded.
   Environment changing faster than agent can adapt = extinction.

### Properties to Derive (not assert as axioms)

- **Model Primacy**: from the structure, show that η* (model quality)
  typically has more leverage than ν (speed) or |A| (action space)
- **Active Testing**: action generates information observation alone cannot
  (Pearl's interventional level). Derivable from information theory.
- **Mismatch as Fundamental Signal**: already in the math as δ_t
- **Destructive Renewal**: when M can't contain a good sufficient statistic,
  need to switch M. Meta-level dynamics.
- **Temporal Nesting**: faster loops nested in slower ones. Faster must
  converge before slower loops act meaningfully.
- **Implicit Precedence**: when model is good, direct model→action (no
  explicit deliberation) outperforms. Deliberation as fallback for novel/
  high-stakes situations.
- **Compression quality**: nature of compression determines capabilities
  and blind spots. Connects to rate-distortion, sufficient statistics.

### Scope / Boundaries

Applies to: agents (any entity that acts on environment and receives feedback)
in open systems under uncertainty.

Does NOT apply (or applies only loosely):
- Pure mathematical structures (no environment, no uncertainty)
- Self-organizing systems without clear agents
- Aesthetic/contemplative experience
- Systems in thermodynamic equilibrium

### Open Mathematical Questions

1. Can η* be genuinely unified across domains or only structurally analogous?
   (55-65% for genuine unification; 30% for single equation)
2. Does the sufficient statistic framing handle destructive renewal cleanly?
   (Probably needs meta-level: form of φ subject to slower-timescale selection)
3. The exact form of the mismatch equation in continuous multi-rate case
4. Whether "model uncertainty" can be made precise for non-parametric models
5. The formal relationship between temporal nesting and convergence constraints

## Relationship to Other Frameworks

### vs. Active Inference / FEP
- We may end up with something that LOOKS like FEP from outside
- But grounded differently: starting from sufficient statistics and
  information arrival, not from free energy minimization as axiom
- Free energy minimization would be a DERIVED property of well-adapted systems
- FEP's own foundations are contested; we want more solid grounding
- Confidence that we'll converge on something FEP-like: ~60%

### vs. TST
- TST is the structural model for HOW we write this
- TST's T-01 (temporal optimality) is analogous to our scope axiom
- TST's T-04 (Bayesian baseline) is analogous to our model-as-sufficient-statistic
- Both build from axioms → definitions → derived theorems
- Both mark epistemic status of each claim
- Both note open questions explicitly

### vs. Boyd's OODA
- Boyd's framework becomes one instantiation
- His deepest insights (Orient dominance, destruction/creation, implicit guidance)
  appear as derived properties, not starting assumptions
- His military applications are one context among many
- His foundational appeals (Gödel, Heisenberg, entropy) acknowledged as
  rhetorically effective but mathematically loose — our theory should do
  what Boyd gestured at but with actual mathematical grounding

## Session 2: TF-03 Formulation, Causality, and Temporal Continuity

### TF-01b: Causal Structure (new axiom)

Pearl's causal hierarchy was elevated from a discussion point in TF-06 to
a foundational axiom (TF-01b). Key design decisions:

- **Causality grounded in temporal ordering** (most primitive notion), NOT
  in statistical influence. The arrow of time — A precedes B — is prior to
  and independent of whether A actually affects B.
- This means the causal structure of the feedback loop is preserved even when
  the agent's coupling to the environment is weak, minimal, or nominal.
  Critical for LLM applications where the agent's "actions" (emitting text)
  may have minimal environmental effect but the temporal ordering still
  structures what can be learned and when.
- Pearl's three levels (associational, interventional, counterfactual) become
  three levels of EPISTEMIC ACCESS available within the loop, grounded in
  the temporal structure rather than in statistical machinery.
- The "causal information yield" decomposition connects exploration (TF-06)
  to the causal axiom: good exploration maximizes the action-contingent
  component of observation information.

### Non-forkability of causal trajectories

TF-01b establishes formally that the interaction history H_t is a unique
temporal sequence, making M_t (as its sufficient statistic) tied to a
SINGULAR causal trajectory. Forking creates divergent histories that
break sufficiency. This has implications for:
- Agent identity and continuity
- Why current LLM architecture (stateless, forkable) is fundamentally
  limited for genuine agency
- The developmental psychology framing from the v7 dialogue (each chat
  as a new "birth" with no continuity)

### Three Questions on TF-03 Formulation

**Q1: Most practical formulation for LLM implementation?**
Answer: Continuous state with event-triggered transitions. The key is the
autonomous orientation term q(M_t) — model evolution BETWEEN events:

  dM/dt = q(M_t) + Σ_k η^(k) · g(δ^(k)) · δ(τ - τ_k)

This is a stochastic differential equation with jumps: continuous drift
(autonomous orientation) plus discrete event-driven updates. The discrete
form is a special case where q = 0 (no interiority between events).

For LLM: this formulation captures "orientation as default mode, message
emission as deliberate action." Between user messages, the agent should be
continuously processing — consolidating, reflecting, retrieving, simulating.
The chat stimulus is an event that triggers an update + action, not the
entirety of the agent's cognitive life.

**Q2: Connection to temporal continuity and consciousness?**
- The autonomous orientation term q(M_t) distinguishes an AGENT from a
  FUNCTION. Functions map inputs to outputs with no state between calls.
  Agents have continuous internal dynamics.
- Temporal continuity (no forking) follows from TF-01b: the model's
  sufficiency is defined relative to a singular causal trajectory.
- Inter-event timing as information: an agent with continuous formulation
  can detect that "the user hasn't responded in 5 minutes" — silence IS
  an observation. Current LLMs cannot represent this.
- Confidence levels:
  - Temporal continuity formally required for model coherence: ~80%
  - Continuous orientation = genuine agency vs. function-calling: ~70%
  - Inter-event timing important and missing from LLMs: ~85%
  - Deep connection to consciousness: ~35-45% (don't overclaim)

**Q3: Should causality be more fundamental?**
Resolved: Yes. Created TF-01b. Causality grounded in temporal ordering
(most primitive) rather than statistical influence. This:
- Tightens the name "Temporal Feedback Theory" (temporal IS the causal structure)
- Survives even with nominal coupling strength
- Gives formal basis for why feedback loops > passive observation
- Connects to general clock-based definition of causality

### Epistemic Audit Results

All TF documents now have explicit epistemic status labels:
- TF-01: Scope Definition (was incorrectly labeled Axiom)
- TF-01b: Axiom (new)
- TF-02: Axiom (correct)
- TF-03: Formulation (was incorrectly labeled Axiom)
- TF-04: Derived (correct)
- TF-05: Derived + Empirical Claim (was just "Derived")
- TF-06: Derived + Discussion (was just "Derived")
- TF-07: Derived + Empirical (was just "Derived")
- TF-08: Derived + Hypothesis (was just "Derived")

## Session 3: Addressing Codex/Gemini Review Feedback

### Changes Made

1. **TF-01b: Fixed causal information decomposition** (Codex flagged as tautological).
   Old: I(o; Ω | M) = I(o; Ω | M, a) + [I(o; Ω | M) - I(o; Ω | M, a)]
   — this was literally X = Y + (X-Y), always true, no content.
   New: Defined Causal Information Yield (CIY) as genuine quantity:
   CIY(a) = I(o; a | M) - I(o; a | Ω, M)
   — total action-observation dependence minus non-causal component.
   CIY = 0 for passive observers; CIY > 0 when actions causally affect environment.

2. **TF-08: Resolved adaptive tempo notation** (Codex flagged).
   Canonical: T = Σ_k ν^(k) · η^(k)*. Single-channel as explicit special case.
   Note that T ≡ ν_eff from TF-03.

3. **TF-06: Deduplicated Pearl's hierarchy** (Codex flagged).
   Now references TF-01b rather than re-deriving.

4. **TF-00: Created notation appendix** (both reviewers recommended).

5. **README: Updated** with TF-00, resolved items, multi-channel T definition.

### Session 3b: Second Review Round + Theorem Pass

Second round of Codex/Gemini feedback (scratch/05, scratch/06) converged on
same priorities: proof discipline, CIY identifiability, TF-03 resolution.

Changes made:

6. **Formal theorem pass** — three propositions added:
   - Prop 4.1 (Mismatch Inevitability): in TF-04. Proves E[|δ|²] > 0
     from TF-01 scope + TF-02 model axiom + S(M) < 1.
   - Prop 7.1 (Structural Adaptation Necessity): in TF-07. Proves
     parametric convergence + low F(M) → floor on mismatch.
   - Prop 8.1 (Persistence Threshold): in TF-08. Solves ODE, shows
     T > ρ necessary for bounded mismatch. Notes robustness beyond
     linear dynamics.

7. **TF-05: Representation layer caveat.** Additive update rule operates
   in transformed space for multiplicative paradigms (Bayesian = additive
   in log-prob space). One paragraph, preserves universality claim.

8. **TF-07: Structural overfitting.** Added opposite failure mode — model
   too expressive, memorizes noise, δ → 0 spuriously. Connects to TF-02
   information bottleneck (marginal complexity cost exceeds marginal
   predictive gain). Structural adaptation is bidirectional.

9. **TF-06.5: Cost of Deliberation.** New document. Derives deliberation
   threshold: Δη* · |δ_post| > ρ · Δτ. Consequences: high-ρ penalizes
   deliberation (formalizes Boyd), diminishing returns → optimal Δτ*,
   implicit action as high-tempo limit.

10. **TF-08: Tensor tempo flag.** Added as Open Question #4. Scalar T
    assumes isotropic adaptive capacity; real systems have dimensional
    variation. Positive-definite condition T - diag(ρ) for full treatment.

### Session 3c: Items 4, 6, 8 + Lyapunov Assessment

11. **TF-01b: CIY identifiability section.** Three regimes: (a) with
    interventional data (standard for active agents — CIY identifiable by
    construction since agents DO things and SEE results; randomized action
    eliminates confounding), (b) observational only (requires DAG or IV
    assumptions — Pearl's fundamental insight restated), (c) practical
    implication: exploration diversity determines CIY estimation quality,
    providing an information-theoretic argument for exploration complementing
    TF-04's mismatch-based argument.

12. **TF-06: Unified policy objective.** Added formal objective:
    π* = argmax[E[value] + λ(M_t) · E[CIY]]. λ depends on U_M, time
    horizon, ρ. Noted structural isomorphism with active inference's
    expected free energy (extrinsic + epistemic value). Key distinction:
    TFT grounds exploration in *causal* information specifically, not
    entropy reduction generally — not all uncertainty reduction is equally
    valuable. Epistemic status: discussion-grade, not yet derived.

13. **TF-08: Strengthened nonlinear dynamics note.** Expanded Open Q #1
    with specific plausible nonlinearities: saturation at large δ,
    threshold/dead-zone effects, structural breakdown. Linear form
    reframed as local Taylor approximation near steady state. Forward
    reference to potential Lyapunov analysis.

### Session 3d: Lyapunov Stability Analysis

14. **scratch/07-lyapunov-stability-analysis.md** — Full working draft.
    Results:
    - L.1 (Bounded Mismatch): Generalizes Prop 8.1 to nonlinear dynamics
      via sector-condition Lyapunov. $R^* = \rho/\alpha$ where α is the
      sector bound (= T in linear case). Clean and complete.
    - L.2 (Stability Margin): Adaptive reserve Δρ* = αR - ρ. Simple
      corollary, immediately useful concept.
    - L.3 (Adversarial Destabilization): A destabilizes B when
      T_A > Δρ*_B / γ_A. Captures asymmetric coupling, finite reserves,
      structural collapse. Strongest and most novel result.
    - L.3.1 (Effects Spiral): Boyd's cascading disorientation as positive-
      feedback Lyapunov instability. Genuinely novel formalization.
    - L.4 (Multi-Timescale): Sketch only. Singular perturbation approach
      for temporal nesting. Needs better specification of slow dynamics G.

    Assessment: L.1-L.3 ready for promotion to TF-08 or appendix.
    L.4 stays in scratch. Key value: persistence and adversarial results
    no longer contingent on linear hypothesis.

    Document also explicitly notes what Lyapunov will NOT illuminate:
    quantitative steady states, convergence rates, optimal gain structure,
    explore-exploit balance, model sufficiency, causal structure.

### Still Pending from Reviews

- TF-03 track decision (minimal async vs. full stochastic process)
- Scope boundaries as first-class document
- Gemini suggestion: reward-as-value-channel
- Citation confidence tiers for v7 references
- Formal derivation of λ(M_t) from first principles (hard — may require
  Bayes-optimal decision theory or Gittins-like analysis)
- Decision: promote L.1-L.3 into TF-08 or create dedicated appendix?

### Notes on Evolution of TF-01

TF-01 is currently a scope definition, not an axiom. If we eventually find
a more irreducible foundational claim (analogous to TST's T-01 tautology),
TF-01 might be displaced. TF-01b (causal structure / arrow of time) is
a candidate — it might be THE foundational axiom, with the scope definition
becoming secondary. But for now the scope-first ordering makes the theory
accessible: "here is what this is about" before "here is the deepest
foundational claim." A more axiomatic TF-01 may emerge with maturity;
we are starting at a good spot with a well-defined "living system."

### Notes on TF-03 Placement

TF-03 (event-driven dynamics) may be premature at position 3. The
discrete-time form is simpler and sufficient for most subsequent derivations.
Options:
  a) Keep at position 3 but make discrete form primary, event-driven as
     generalization (current draft leans event-driven-primary)
  b) Move later (after TF-05 or TF-06) as "the discrete notation is a
     special case of this more general formulation"
  c) Split: put the continuous+jumps formulation (with q(M_t) term) in a
     separate TF that explicitly addresses LLM-relevant "continuous interiority"

Option (c) has appeal because it separates the engineering convenience
(multi-rate events) from the deep philosophical content (autonomous
orientation between events). The former is TF-03; the latter might be
a new TF or part of the LLM strategy document.

## File Organization

- ~/src/temporal-feedback/ — working directory (git repo)
- ~/src/temporal-feedback/scratch/ — intermediate work, notes
- ~/src/temporal-feedback/ooda-loop-universal-pattern-v7.md — source material
- Final documents: TF-01.md, TF-01b.md, TF-02.md, etc. (in root of temporal-feedback/)

- ~/src/temporal-feedback/ — working directory (git repo)
- ~/src/temporal-feedback/scratch/ — intermediate work, notes
- ~/src/temporal-feedback/ooda-loop-universal-pattern-v7.md — source material
- Final documents: TF-01.md, TF-02.md, etc. (in root of temporal-feedback/)
