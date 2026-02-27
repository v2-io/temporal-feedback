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

## File Organization

- ~/src/temporal-feedback/ — working directory (git repo)
- ~/src/temporal-feedback/scratch/ — intermediate work, notes
- ~/src/temporal-feedback/ooda-loop-universal-pattern-v7.md — source material
- Final documents: TF-01.md, TF-02.md, etc. (in root of temporal-feedback/)
