# Consolidated Pending Items (2026-02-28)

All unfixed items from the combined Claude/Codex/Gemini audit (Session 5), plus anything still pending from earlier scratch feedback files (03, 04, 05, 06) and scratch/00. Items marked with source(s) for traceability.

---

## A. Proof / Formal Rigor

**A1. TF-04 Prop 4.1 marginalization gap** (Claude audit A2)
- The proof sketch jumps from "M is not sufficient" to "prediction differs from true conditional mean" without showing the marginalization step explicitly.
- Severity: Presentation, not correctness. The claim is true but the proof should show *why* insufficient statistics yield biased conditional expectations.
- Fix: Add one line showing E[o|M] != E[o|Omega] when M loses information, via data processing inequality or Jensen's inequality.

**A2. TF-06.5 Prop 6.5.1 endogenous |delta_post|** (Claude audit A3)
- The proposition treats |delta_post| (post-deliberation mismatch) as exogenous, but it depends on the quality of deliberation, which depends on M_t, which is what we're evaluating.
- Severity: The result is qualitatively correct (deliberation has diminishing returns and a time cost) but the formal statement is imprecise.
- Fix: Either (a) condition on a fixed delta_post and state the result as "given deliberation achieves this improvement," or (b) model delta_post as a function of Delta_tau with explicit assumptions about the deliberation process.

**A3. TF-02 axiom justification tightness** (Claude audit B2)
- TF-02 asserts the model axiom with motivation but could invoke TF-01's constraints more directly — showing that the axiom is *forced* by the scope definition rather than just plausible.
- Severity: Low. The axiom is well-motivated. But tighter coupling to TF-01 would strengthen the derivation chain.

**A4. Formal derivation of lambda(M_t)** (scratch/00)
- The exploration-exploitation weight in TF-06's unified policy objective is currently discussion-grade. Deriving it from first principles likely requires Bayes-optimal decision theory or Gittins-like analysis.
- Severity: Medium. The structural form is plausible but the claim that lambda depends on U_M, time horizon, and rho is asserted, not derived.
- Difficulty: Hard. May be the single hardest open formalization in the theory.

**A5. Structural adaptation threshold formalization** (scratch/04, scratch/06)
- TF-07 identifies *when* structural adaptation is needed (persistent irreducible mismatch) but doesn't formalize the *threshold* — how much evidence of model class inadequacy justifies the cost of structural change.
- The information bottleneck provides a natural criterion (marginal predictive gain per marginal complexity = 0), noted in TF-07's structural overfitting section, but not elevated to a formal proposition.
- Fix: Consider a Prop 7.2 stating the IB-based threshold condition explicitly.

---

## B. Notation / Typing / Consistency

**B1. Universal gain language** (Codex medium #1)
- TF-05 states eta* = U_M/(U_M+U_o) as if it's a universal law, but for non-conjugate models and neural networks, "U_M" isn't well-defined as a scalar.
- The document does discuss this in Open Questions, but the main claim's language could be more carefully qualified upfront.
- Fix: Add a brief caveat near the main claim (not just in Open Questions) noting that the structural form is the claim, and the scalar ratio is the simplest instantiation.

**B2. Axiom dependency order / dependency map** (Codex medium #3)
- The theory would benefit from an explicit dependency diagram showing which results depend on which axioms/definitions.
- Fix: Add to README or TF-00 a simple dependency DAG: TF-01 -> TF-01b -> TF-02 -> TF-03 -> TF-04 -> TF-05 -> TF-06 -> etc., with cross-links.

**B3. TF-05 missing aggregate confidence** (Claude audit D2)
- TF-05 discusses domain validation case by case but doesn't summarize overall confidence in the universal gain claim. Comparable to the confidence estimates in other documents.
- Fix: Add a brief "Overall assessment" paragraph with a confidence estimate.

**B4. CIY threading through TF-05** (scratch/06)
- CIY is defined in TF-01b, used in TF-06's policy objective, but not explicitly connected to TF-05's gain discussion. The gain on an observation channel should relate to the CIY of the action that generated that observation.
- Fix: Add a brief note in TF-05 connecting channel-specific gain to the CIY framework.

---

## C. Architectural / Structural Decisions

**C1. TF-03 track decision** (scratch/00, scratch/03, scratch/04, scratch/06)
- Unresolved: should TF-03 be:
  - Track A: Minimal async event formalism (current lean), with continuity/interiority as separate extension
  - Track B: Full stochastic process with explicit drift/jump terms as primary formulation
  - Track C (scratch/00): Split — multi-rate events stay in TF-03; continuous interiority / "thinking mode" becomes a new TF
- This is the longest-standing unresolved architectural decision.

**C2. Scope boundaries as first-class document** (scratch/00, scratch/03, scratch/06)
- The v7 boundaries section is strong but lives in the source material, not in the core TF chain.
- Risk: without boundaries in the core set, the universality claim appears unfalsifiable.
- Fix: Create a dedicated TF appendix (Appendix B?) or short TF document explicitly stating where TFT does NOT apply and why.

**C3. Core Theorems vs Discussion Claims index** (Codex next steps)
- The theory mixes formal propositions (4.1, 7.1, 8.1, 6.5.1, A.1-A.3) with discussion-grade claims throughout.
- An index distinguishing "proven under stated assumptions" from "argued but not formally derived" would help reviewers.
- Fix: Add a table to README or TF-00.

---

## D. Expansion Suggestions (Lower Priority)

These are not gaps in the current theory but directions worth considering. Included here so they aren't lost.

**D1. Continuous interiority / "thinking mode"** (scratch/04 Gemini, scratch/00)
- Define internal observations o_int generated by M updating itself.
- Chain of thought / inference-time compute as internal feedback loop increasing eta* before external action.
- The q(M_t) autonomous orientation term in TF-03's SDE-with-jumps formulation is the mathematical home for this.
- Closely tied to C1 (TF-03 track decision).

**D2. Reward-as-value-channel** (scratch/04 Gemini)
- In a truly universal theory, reward should not be a special primitive. Instead, the agent has a "value channel" (a specific o^(k)) predicted like any other channel.
- Mismatch on the value channel drives policy improvement; mismatch on state channels drives world-model improvement.
- This would more cleanly unify RL and active inference under TFT.

**D3. Formal metric for action fluency** (Gemini session 5)
- Action fluency is currently a qualitative property. A formal metric would strengthen TF-06.
- Possible: something like the ratio of action quality to deliberation time, or the mutual information between M_t and optimal action without search.

**D4. Distributed / Social TFT** (Gemini session 5)
- Multi-agent systems where agents are both observation channels and action targets for each other.
- The adversarial section (TF-06, TF-08, Appendix A.3) covers the competitive case. The cooperative case (collective model building, distributed sufficient statistics) is unexplored.
- May be better suited for a follow-on paper than the current document.

**D5. Zero-mismatch ambiguity expansion** (Gemini session 5)
- TF-04 notes that delta ~= 0 is ambiguous (accurate model vs. insufficient exploration vs. low channel resolution).
- Could expand with a formal "novelty signal" or "surprise-potential" metric that measures expected information from untested regions.
- Connected to CIY: regions with high expected CIY but no recent probes are candidates for exploration.

**D6. Tensor tempo formalization** (scratch/04, flagged as TF-08 Open Q #4)
- Currently noted as an open question. Full treatment would define T as a positive-definite matrix with persistence condition T - diag(rho) > 0 (positive-definite).
- Captures dimensional decoupling: high tactical tempo but low strategic tempo.

**D7. Citation confidence tiers for v7** (scratch/03, scratch/06)
- The v7 source material cites a wide range from peer-reviewed to blog posts.
- Triage into Tier 1 (primary/archival), Tier 2 (credible secondary), Tier 3 (exploratory/commentary).
- Important for eventual publication but not blocking for theory development.

---

## E. Larger-Scale Next Steps

**E1. One fully worked end-to-end domain instantiation** (Codex next steps)
- Pick one domain (Kalman filter is cleanest) and trace the ENTIRE theory through it:
  TF-01 scope -> TF-01b causal structure -> TF-02 model -> TF-03 events -> TF-04 mismatch -> TF-05 gain -> TF-06 action -> TF-07 structural -> TF-08 tempo -> Appendix A stability.
- This would be the strongest validation of the theory's coherence.

**E2. Operationalization / estimation recipes** (Codex medium #4)
- For each main quantity (S(M_t), eta*, CIY, T, rho), provide concrete estimation procedures in at least 2-3 domains.
- Currently the theory defines these quantities but doesn't always say how to measure them.

**E3. Deliverable #2: LLM training strategy** (scratch/00)
- Not yet started. This is the practical application document grounding TFT in LLM design.
- Should be informed by the continuous interiority discussion (D1), reward-as-value-channel (D2), and the non-forkability result from TF-01b.

---

## Cross-Reference: What HAS Been Done

For completeness, here are the major items from earlier feedback that have been resolved:

- Theorem pass: Props 4.1, 7.1, 8.1, 6.5.1, A.1-A.3 (scratch/03, 05, 06)
- TF-00 notation appendix (scratch/03, 04, 06)
- CIY repair in TF-01b (scratch/03)
- CIY identifiability section (scratch/06)
- TF-05 representation layer caveat (scratch/05, 06)
- TF-06.5 deliberation cost (scratch/05)
- TF-07 structural overfitting (scratch/05)
- TF-06 unified policy objective (scratch/05, 06)
- TF-08 nonlinear dynamics + Lyapunov appendix (scratch/04, 05)
- TF-08 tensor tempo flagged as open Q (scratch/04)
- TF-06 query actions section (Session 4b)
- TF-06 adversarial mirror section (Session 4c)
- Persistence threshold rewrite (Codex #1)
- Dimensional analysis fix (Codex #2)
- Epsilon direction fix (Codex #4)
- F -> Phi symbol collision (Codex medium, Claude C2)
- CIY non-negativity noted (Codex #3)
- Level 2/3 conflation fix (Claude A1)
- Various language/attribution fixes (Claude B1, B3, B4, D1, D3, D4)
- H_t attribution, nu_eff addition (Claude C1, C4)
- Game theory gap noted in README (Claude E3)
- Action fluency concept (Session 4)
- Multi-timescale continuum rewrite of TF-07 (Session 4)
- N-timescale Appendix A.4 (Session 4)
- Modal language pass (Session 4)
