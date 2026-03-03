# TFT Slide Deck — Outline

## Narrative Arc
The deck tells the story of how adaptive agents persist under uncertainty.
It starts with the question "what does every adaptive system have in common?"
and builds toward the formal answer: the feedback loop, its quantities, and
its stability conditions.

## Part I: Foundation (Slides 1–8)
1. **Title slide** — Temporal Feedback Theory: The Universal Structure of Adaptive Intelligence
2. **The question** — What do thermostats, fighter pilots, immune systems, and AI agents have in common?
3. **Scope** — Agent ↔ Environment under Uncertainty (TF-01). The lossy observation, the action that changes the world, the residual uncertainty that never goes away.
4. **The temporal arrow** — Time flows one way (TF-02). Actions precede consequences. This is the foundation of causality, not a convenience.
5. **Three levels of knowing** — Pearl's causal hierarchy grounded in temporal structure (TF-02). Association → Intervention → Counterfactual.
6. **The Model** — Every agent compresses its history into a working model (TF-03). The sufficient statistic. M_t = φ(H_t).
7. **The information bottleneck** — Compression vs. prediction (TF-03). The Pareto frontier. PID's R³ vs. Bayesian posterior.
8. **Events on channels** — Multi-rate, multi-channel dynamics (TF-04). Not clock ticks but events arriving at different rates.

## Part II: The Core Engine (Slides 9–15)
9. **The mismatch signal** — Prediction meets reality (TF-05). δ_t = o_t - ô_t. The fundamental driver.
10. **Mismatch inevitability** — Prop 5.1. It never goes away. Decomposition: model error (reducible) + noise (irreducible).
11. **The zero-mismatch trap** — Low mismatch doesn't mean good model (TF-05). The three cases: accurate, blind, or low-resolution.
12. **The update gain** — How much to trust the new vs. the old (TF-06). η* = U_M/(U_M + U_o). The uncertainty ratio principle.
13. **Gain across domains** — Kalman gain, Bayesian posterior, RL learning rate, PID tuning (TF-06). Same structure, different names.
14. **Gain dynamics** — Convergence, reset, overfitting as miscalibration (TF-06).
15. **The feedback loop complete** — Perceive → Orient (predict + surprise + gain) → Decide → Act. The loop that every adaptive system runs.

## Part III: Action and Deliberation (Slides 16–20)
16. **Action as model function** — a_t = π(M_t) (TF-07). Implicit (reflexive) vs. explicit (deliberative).
17. **Action fluency** — The spectrum from reflex to deliberation (TF-07). When System 1 vs. System 2. Evolutionary pressure toward internalization.
18. **Actions generate information** — CIY and exploration (TF-08). Acting to learn, not just to achieve. Query actions: asking an expert vs. probing the world.
19. **The cost of deliberation** — Time spent thinking is time the world moves (TF-09). Prop 9.1: deliberation threshold. When to think vs. when to act.
20. **The deliberation spectrum** — From zero deliberation (reflex) to massive deliberation (structural change). All governed by ρ_delib × Δτ.

## Part IV: Structure and Change (Slides 21–24)
21. **Structural adaptation** — When the model class itself is wrong (TF-10). Prop 10.1: persistent structured residuals → change the framework.
22. **Destruction and creation** — Boyd's snowmobile. Decompose, recombine, expand, graft (TF-10). The six-phase cycle.
23. **Structural overfitting** — The opposite failure: too complex, fitting noise (TF-10). The information bottleneck as diagnostic.
24. **The cost of structural change** — Massive deliberation cost (TF-10 via TF-09). Why conservatism is rational. When to pay the price.

## Part V: Tempo, Persistence, and Adversarial Dynamics (Slides 25–34)
25. **Temporal nesting** — Fastest to slowest: reactive → parametric → structural → architectural (TF-11). The convergence constraint.
26. **Adaptive tempo** — T = Σ ν^(k) · η^(k)* (TF-11). Rate × Quality. The single number that captures adaptive fitness.
27. **Speed-quality substitutability** — Doubling quality = doubling speed (TF-11). Boyd's insight formalized.
28. **The mismatch dynamics** — d‖δ‖/dt = -T·‖δ‖ + ρ (TF-11). The ODE. Steady state: ‖δ‖_ss = ρ/T.
29. **The persistence threshold** — T > ρ/‖δ_critical‖ (Prop 11.1). The line between survival and extinction.
30. **Beyond linearity: Lyapunov** — The sector condition (App A). Bounded mismatch under general nonlinear dynamics (Prop A.1).
31. **Adaptive reserve** — Δρ* = αR - ρ (Prop A.2). Shock tolerance. Robust vs. fragile.
32. **Adversarial dynamics** — A's tempo becomes B's disturbance (TF-11). Squared tempo advantage (Cor 11.2).
33. **Adversarial destabilization** — γ_A · T_A > Δρ*_B (Prop A.3). Pushing past the stability boundary.
34. **The effects spiral** — Positive-feedback Lyapunov instability (Cor A.3.1). Boyd's cascading disorientation, derived.

## Part VI: Multi-Agent and Communication (Slides 35–38)
35. **Communication gain** — Extending η* to social channels (App F). η*_ji with source quality and alignment uncertainty.
36. **Trust as meta-model** — Calibrating who to trust about what (App F). Bayesian trust, transitive trust, risk-adjusted trust.
37. **Distributed tempo** — Team persistence (App F). Cooperative coupling reduces effective ρ. Teams can persist where individuals cannot.
38. **Topology matters** — Peer networks, ensembles, hierarchies (App F). Groupthink as gain collapse. Micromanagement as convergence violation.

## Part VII: Validation and Application (Slides 39–42)
39. **Kalman: exact validation** — End-to-end worked example (App C). Every TFT quantity has a Kalman counterpart.
40. **RL: approximate validation** — Nonstationary bandit (App D). Where Q-learning is TFT-deficient. Per-dimension persistence failure.
41. **Cross-domain mapping** — The universal pattern table. Same structure in control, biology, organizations, military, science, immune.
42. **Falsification predictions** — Five testable predictions (README). What would kill the theory.

## Part VIII: Closing (Slides 43–44)
43. **What TFT is and isn't** — A formal vocabulary for adaptive systems. Not a grand unified theory but a structural template with mathematical teeth.
44. **The open frontier** — Non-parametric gain, tensor tempo, continuous interiority, game-theoretic communication, thermodynamic connections.
