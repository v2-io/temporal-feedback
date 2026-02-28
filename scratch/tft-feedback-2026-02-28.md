# Temporal Feedback Theory Draft Review

Date: 2026-02-28
Corpus reviewed (in requested order): `README.md`, `TF-00.md` through `TF-11.md`, `Appendix-A-Lyapunov.md`

## Executive Assessment

This draft is unusually strong for an early-stage theory document. The core architecture is coherent, the epistemic-status labeling is explicit, and the cross-domain mapping is thoughtful rather than superficial. The strongest contribution is the combination of:

1. A clean "agent under uncertainty" framing with explicit scope.
2. A reusable mismatch/gain structure that unifies many practical systems.
3. A credible attempt to upgrade the linear tempo claim into a nonlinear stability treatment via Lyapunov methods.

The biggest risk is not "wrong math" so much as **category drift** between levels of claim (axiom/derived/hypothesis/discussion) and between operational quantities and conceptual proxies. Right now, some powerful ideas are present but can still be challenged as "good analogies with partial formal glue." Tightening a small set of interfaces would move this from strong draft to very defensible framework.

## High-Impact Findings (Priority Order)

### 1) Policy-objective location is split between TF-07 and TF-08

- `TF-00.md:181` indexes the unified policy objective under TF-07.
- The explicit objective equation appears in `TF-08.md:11`.
- `TF-06.md:108` defers CIY-gain linkage to TF-08, which supports TF-08 as canonical location.

Impact:
- Readers tracking formal claims by index can mis-locate a central equation.
- Creates uncertainty about whether TF-08 is optional discussion or core formal hypothesis.

Fix:
- Update `TF-00.md:181` to reference TF-08.
- Add one sentence in TF-07 saying the formal objective is in TF-08.

### 2) CIY is conceptually central but not mathematically "closed" yet

Current state:
- CIY is introduced in TF-02 as a difference of mutual informations (`TF-02.md:88`).
- Non-negativity caveat is acknowledged (`TF-02.md:96`) and notation notes this (`TF-00.md:27`).
- Estimation/identifiability limits are discussed (`TF-02.md:100-115`).

Impact:
- This is responsible scholarship, but CIY currently acts like both a formal object and an intuition pump.
- It is difficult to use CIY in downstream equations without a strict admissible regime.

Fix:
- Add a formal "CIY admissibility box":
  - Regime A: randomized interventions (`CIY = I(o_t; a_{t-1} | M_{t-1})`, non-negative by construction).
  - Regime B: observational with assumptions (DAG/IV/etc), CIY estimate carries identifiability qualifier.
  - Regime C: adversarial communication, use "effective CIY" as a separate quantity.
- This would prevent mixing causally identified CIY with trust-mediated communicative effects.

### 3) TF-09 depends on TF-11 hypothesis; this is honest but easy to attack

Current state:
- `TF-09.md:5` states derived from TF-11 mismatch dynamics.
- TF-11 clearly labels mismatch ODE as hypothesis (`TF-11.md:5`, `TF-11.md:77`).

Impact:
- Critics can claim Proposition 9.1 is "derived from a derived result," when it is actually conditional on a hypothesis.

Fix:
- Re-label Proposition 9.1 as: "derived conditional on TF-11 H1."
- In TF-00 results index (`TF-00.md:168`), add "(conditional on TF-11 hypothesis)".

### 4) The scalar tempo shorthand is productive but still semantically overloaded

Strength:
- You already addressed dimensional consistency and normalized condition (`TF-00.md:135`, `TF-11.md:90-92`).

Remaining issue:
- Text still repeatedly states `\mathcal{T} > \rho` without immediate reminder that it is normalized shorthand (`TF-11.md:5`, `TF-11.md:94`, `TF-11.md:96`, `TF-11.md:141`).

Impact:
- New readers will misread this as raw-unit inequality and infer unit inconsistency.

Fix:
- Standardize one notation:
  - raw: `\mathcal{T} > \rho / |\delta_critical|`
  - normalized: `\tilde{\mathcal{T}} > \tilde{\rho}`
- Use normalized only after introducing tildes once per document.

### 5) Mismatch decomposition proof language can be tightened

Current state:
- TF-05 decomposition is plausible (`TF-05.md:37-39`) and basically standard.
- Wording says cross-term vanishes due to mean-zero and independence (`TF-05.md:39`).

Impact:
- "Independence" claim is stronger than required and could invite technical objection.

Fix:
- Replace with orthogonality via conditional expectation/tower property wording.
- This is a minor edit that preempts unnecessary debate.

## Medium-Priority Findings

### 6) Epistemic-status taxonomy is good, but some headings and body labels diverge

- TF-08 title labels as "Hypothesis" while body says "structural claim is discussion-grade" (`TF-08.md:1`, `TF-08.md:5`).

This is not wrong, but readers may ask:
- Is TF-08's objective equation a hypothesis, or discussion with external support?

Fix:
- Pick one:
  - "Hypothesis + Discussion" in heading; or
  - Keep "Hypothesis" and explicitly tag equation as H8.1.

### 7) Query-action section is high-value but should be separated into cooperative vs adversarial formal channels

Strength:
- Best conceptual section in the draft for practical modern AI/social systems (`TF-08.md:50-86`).

Issue:
- Cooperative query CIY and deceptive-channel effects currently share vocabulary ("negative effective CIY") that blurs interpretation.

Fix:
- Introduce explicit decomposition:
  - Environment probe channel.
  - Communication channel with source model `M_source`.
  - Trust/reliability estimator `\tau_source`.
- Then define expected update value as function of both CIY and reliability-weighted distortion risk.

### 8) Appendix A assumptions are excellent; tie assumption failure to TF-10 trigger more explicitly

Strength:
- A2' local sector condition and region radius `R` connect naturally to model-class limits (`Appendix-A-Lyapunov.md:80-84`, `:90`).

Opportunity:
- Add a brief "Assumption Failure Map":
  - A2' fails due to saturation -> parametric slowdown.
  - A2' fails due to representational mismatch -> structural adaptation required.

This would make TF-10 and Appendix A feel fully stitched rather than adjacent.

### 9) Some strong assertions could use "scope guardrails" to avoid over-claim

Examples:
- "Any persisting agent maintains a model" (`TF-03.md:3`) is intended as definitional/tautological in scope.
- Readers outside your intended ontology may interpret this as metaphysical universal claim.

Fix:
- Add one short sentence:
  - "In TFT, 'model' includes any internal state that induces non-random action-history dependence; this is a representational definition, not a psychological claim."

### 10) Document-scale readability: repeated domain tables are useful but slightly diluting

Strength:
- Cross-domain mappings are one of the major utilities of this project.

Issue:
- Repeated table patterns increase length and may reduce theorem-chain focus for technical readers.

Fix:
- Keep one compact table per TF, move exhaustive domain mappings to a dedicated appendix (already identified in README missing items).

## Utility, Form, Function

### Utility

High utility as:
- A unification lens for control/learning/decision systems.
- A design heuristic for AI agents (especially in online and adversarial settings).
- A conceptual bridge between engineering and strategy (Boyd, organizational learning, RL, estimation).

Current strongest practical takeaways:
- Mismatch is inevitable and should be managed, not eliminated.
- Gain calibration is central and cross-domain.
- Tempo is product-like (rate x quality), not just speed.
- Structural adaptation and overfitting are dual failure modes.

### Form

Strong:
- Modular progression with explicit dependency graph (`TF-00.md:137-158`).
- Epistemic labels throughout.
- Honest open-questions sections.

Needs tightening:
- Consistent claim placement/indexing.
- Cleaner separation of formal objects vs interpretive extensions.
- Standardized shorthand notation to avoid repeated clarifications.

### Function

What it currently does well:
- Generates coherent explanations across many domains.
- Supports strategic diagnosis ("why are we brittle?", "when should we re-architect?").
- Provides a scaffold for future formalization and experiments.

What it does not yet do:
- Offer a complete testable quantitative package across domains.
- Provide fully operational definitions for all headline quantities in high-dimensional settings (`U_M`, CIY under constraints, tensor tempo).
- Resolve communication/game-theoretic trust dynamics formally.

Interpretation:
- This is currently best viewed as a **research program with several proved local theorems**, not a closed final theory.
- That is a good state for this stage; the draft is unusually explicit about what is and is not proven.

## Recommendations for Next Revision Cycle

1. Build a "formal core" document of only axioms, definitions, propositions, assumptions, and proofs.
2. Keep current rich domain/explanatory text as companion narrative docs.
3. Add a single symbol/assumption registry table with global tags (`A2'`, `H11.1`, etc.) and references.
4. Promote one or two empirical validation templates:
   - synthetic control system benchmark;
   - RL nonstationary benchmark;
   - adversarial communication simulation.
5. Add a minimal "how to falsify key claims" section to increase scientific credibility.

## Suggested Micro-Edits (Low Effort, High Return)

- `TF-00.md:181`: point unified policy objective to TF-08.
- `TF-05.md:39`: replace "independent" with orthogonality/conditional expectation language.
- `TF-09.md:5` and `TF-00.md:168`: mark conditional dependence on TF-11 hypothesis explicitly.
- `TF-11.md`: define normalized variables once and consistently use them for threshold shorthand.
- `TF-08.md`: split cooperative query gains from adversarial deception dynamics with explicit channel notation.

## Bottom Line

This is a high-quality theory draft with real integrative power. The conceptual architecture is strong enough to justify continued formal investment. The next quality jump will come from reducing interface ambiguity:

- where each major claim lives,
- under exactly which assumptions each proposition is valid,
- and which quantities are currently formal vs heuristic.

If those interfaces are tightened, this framework can move from "compelling synthesis" to "credible formal backbone plus practical design doctrine."
