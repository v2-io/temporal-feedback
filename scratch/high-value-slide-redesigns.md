# High-Value Slide Redesigns

These six slides replace current Slides 13, 23, and 24 with pairs that
build visual mental models stepwise, then map notation onto them.

---

## REDESIGN 1: THE GAIN (replaces current Slide 13)

### Slide 13a: The Uncertainty Ratio — Visual

# THE UNCERTAINTY RATIO

### TF-06 — What the Gain Looks Like

Imagine a bar representing **total uncertainty** about the next observation:

$$\underbrace{\quad U_M \quad}_{\text{model uncertainty}} + \underbrace{\quad U_o \quad}_{\text{observation uncertainty}} = \text{total uncertainty}$$

The gain $\eta^*$ is simply **the fraction that belongs to the model**:

$$\eta^* = \frac{U_M}{U_M + U_o}$$

**Visualization**: A single horizontal bar divided into two colored segments. The LEFT segment is orange, labeled $U_M$ — the model's own uncertainty about what it's about to see (how spread out the model's predictive distribution is). The RIGHT segment is dark/gray, labeled $U_o$ — the noise in the observation channel (how much the channel distorts the signal). The total bar length is $U_M + U_o$.

Above the bar: a bracket spanning only the orange segment, labeled "$\eta^*$" with the value shown as a fraction of total bar length.

Three versions of the bar shown vertically, like a stacked progression:

**Top bar**: $U_M$ is ~80% of the bar, $U_o$ is ~20%. $\eta^* \approx 0.8$. Label: "Model very uncertain, observation clean → **trust the data**"

**Middle bar**: $U_M$ is ~50%, $U_o$ is ~50%. $\eta^* \approx 0.5$. Label: "**Split the difference**"

**Bottom bar**: $U_M$ is ~15%, $U_o$ is ~85%. $\eta^* \approx 0.15$. Label: "Model confident, observation noisy → **trust the model**"

The key visual: $\eta^*$ is always the orange fraction. As the orange segment shrinks (model becomes more confident), $\eta^*$ shrinks — the model becomes more resistant to individual observations. As it grows (model becomes uncertain), $\eta^*$ grows — the model becomes more responsive to data.

> **Notation**: $U_M$ (model uncertainty) is the variance — the spread — of the model's own predictive distribution. Formally: $U_M = \text{Var}_{M_{t-1}}[\hat{o}_t \mid a_{t-1}]$ — how spread out the model's predictions are. When $U_M$ is large, the model "doesn't know what's coming." When small, the model has a precise expectation. $U_o$ (observation uncertainty) is the variance of the observation noise $\varepsilon_t$ — how much the channel distorts the true signal. Together they define total uncertainty. The gain $\eta^*$ is the fraction of total uncertainty that belongs to the model — geometrically, it's the orange proportion of the bar. This single visual encodes the formula: the ratio IS the proportion. When the orange is most of the bar, trust the data. When the gray is most, trust the model.

---

### Slide 13b: The Gain as Movement — Visual

# THE GAIN AS MOVEMENT

### TF-06 — What the Update Looks Like

The update rule:

$$M_t = M_{t-1} + \eta^* \cdot g(\delta_t)$$

In one dimension, this is **movement toward the observation**:

$$M_t = M_{t-1} + \eta^* \cdot (o_t - \hat{o}_t)$$

**Visualization**: A horizontal number line. Two points are marked:

- **Left point** (orange dot): $M_{t-1}$ — the model's current position (its prediction $\hat{o}_t$)
- **Right point** (dark dot): $o_t$ — where reality actually landed

The gap between them is labeled $\delta_t = o_t - \hat{o}_t$ (the mismatch).

Below the number line: a new orange dot at position $M_t$ — the updated model. Its position is:
- When $\eta^* = 1$: $M_t$ jumps all the way to $o_t$. Arrow: "Full trust in data."
- When $\eta^* = 0.5$: $M_t$ moves halfway. Arrow: "Split."
- When $\eta^* = 0.1$: $M_t$ barely moves. Arrow: "Mostly trust the model."

Show all three as stacked number lines, each with the same $M_{t-1}$ and $o_t$ but different $M_t$ positions — governed by η*.

The visual literally shows: **η* is the fraction of the gap the model closes toward the observation.**

Connect to the bar from 13a: include a small version of the uncertainty-ratio bar next to each number line, showing the corresponding $U_M$/$U_o$ partition that produces that η*.

> **Notation**: $M_t = M_{t-1} + \eta^* \cdot g(\delta_t)$ says the new model is the old model shifted by a gain-weighted correction. In 1D: the model moves toward the observation by fraction $\eta^*$ of the mismatch $\delta_t$. $g(\delta_t)$ is the mismatch transform — in the simplest case it's just $\delta_t$ itself; in higher dimensions it maps from observation space to model space (in a Kalman filter, this is $H^T$, the transpose of the observation matrix). The visual encodes the formula: $\eta^*$ controls how far the model moves. Near 1 → jumps to the data. Near 0 → barely budges. This movement is the same thing shown by the bar partition in the previous slide: when the model is uncertain (large orange segment), it moves far; when confident (small orange segment), it resists.

---

## REDESIGN 2: THE MISMATCH ODE + PERSISTENCE (replaces current Slide 23)

### Slide 23a: Two Competing Forces — Visual

# TWO COMPETING FORCES

### TF-11 — How Mismatch Evolves

$$\frac{d\|\delta\|}{dt} = -\underbrace{\mathcal{T} \cdot \|\delta\|}_{\text{correction}} + \underbrace{\rho}_{\text{disturbance}}$$

**Visualization**: A horizontal axis representing mismatch magnitude $\|\delta\|$, running from 0 (left, "perfect model") to large (right, "badly wrong model"). A small ball sits at some position on this axis.

Two forces act on the ball:

**LEFTWARD ARROW** (orange, labeled "Correction: $\mathcal{T} \cdot \|\delta\|$"): This arrow points LEFT (toward zero mismatch). Its length is proportional to both $\mathcal{T}$ (tempo) and the ball's current position $\|\delta\|$. Crucially: **the arrow gets longer when the ball is further right** — larger errors produce stronger correction. At position 0, the arrow vanishes.

**RIGHTWARD ARROW** (dark, labeled "Disturbance: $\rho$"): This arrow points RIGHT (toward larger mismatch). Its length is **constant** — the environment changes at rate $\rho$ regardless of the model's current state.

Show three snapshots of the ball at different positions:

**Position A** (far right, large $\|\delta\|$): Correction arrow is MUCH longer than disturbance arrow. Net force: strongly leftward. Ball moves left. Label: "$\mathcal{T}\|\delta\| > \rho$ → mismatch decreasing."

**Position B** (at $\|\delta\| = \rho/\mathcal{T}$): Correction arrow equals disturbance arrow exactly. Net force: zero. Ball stationary. Label: "**STEADY STATE**: $\|\delta\|_{ss} = \rho/\mathcal{T}$"

**Position C** (near zero, small $\|\delta\|$): Correction arrow is shorter than disturbance arrow. Net force: rightward. Ball moves right. Label: "$\mathcal{T}\|\delta\| < \rho$ → mismatch increasing."

The key insight this teaches: the steady state is **self-correcting**. If the ball is too far right, the correction dominates and pulls it back. If too far left, the disturbance dominates and pushes it right. It always settles at $\rho/\mathcal{T}$.

> **Notation**: $d\|\delta\|/dt$ is the instantaneous rate of change of mismatch magnitude — how fast the ball is moving. The double-bars $\|\delta\|$ denote the norm (length/magnitude) of the mismatch vector. $\mathcal{T}$ is adaptive tempo (rate × quality, from Slide 22). The correction term $\mathcal{T} \cdot \|\delta\|$ is proportional to both tempo AND current mismatch — this is why the leftward arrow grows with position. It means larger errors get corrected faster, which is the self-stabilizing mechanism. $\rho$ (rho) is the environment change rate — measured in surprise per unit time — the rate at which reality diverges from the model due to environmental change. It's constant (doesn't depend on the current mismatch). The steady state $\|\delta\|_{ss} = \rho/\mathcal{T}$ is where the ball settles — the ratio of disturbance to adaptation. Higher tempo → lower steady-state error. Higher disturbance → higher steady-state error.

---

### Slide 23b: The Persistence Threshold — Visual

# THE PERSISTENCE THRESHOLD

### Proposition 11.1

$$\boxed{\mathcal{T} > \frac{\rho}{\|\delta_{\text{critical}}\|}}$$

**Visualization**: The same horizontal axis from Slide 23a (mismatch magnitude, 0 to large). The ball-and-arrows picture is retained but now with a **critical line** — a vertical dashed red line at position $\|\delta_{\text{critical}}\|$, dividing the axis into two zones:

**LEFT ZONE** (green background): "MODEL WORKS" — mismatch is small enough for effective action.

**RIGHT ZONE** (red background): "MODEL FAILS" — mismatch is too large; the agent is acting on stale/wrong information.

Now show THREE scenarios (three copies of the axis, stacked):

**Scenario 1**: High $\mathcal{T}$. The steady-state ball position $\rho/\mathcal{T}$ is well left of the critical line. PERSIST. Label: "High tempo → low steady-state mismatch → model works."

**Scenario 2**: Moderate $\mathcal{T}$. The ball sits just left of the critical line. MARGINAL. Label: "Barely adequate tempo → mismatch near the edge."

**Scenario 3**: Low $\mathcal{T}$. The ball sits right of the critical line. FAIL. Label: "Tempo too low → steady-state mismatch exceeds threshold → model useless."

The threshold formula: the critical line is at $\|\delta_{\text{critical}}\|$. The ball is at $\rho/\mathcal{T}$. Persistence requires $\rho/\mathcal{T} < \|\delta_{\text{critical}}\|$, which rearranges to $\mathcal{T} > \rho/\|\delta_{\text{critical}}\|$.

Below: four domain instances where the threshold is crossed:
- **Extinction**: environment changes faster than organism adapts
- **Organizational failure**: market moves faster than company learns
- **Control instability**: disturbances exceed controller capacity
- **Cognitive overload**: information faster than processing

> **Notation**: $\|\delta_{\text{critical}}\|$ is the critical mismatch threshold — the magnitude beyond which the model can no longer support effective action. This is domain-specific: in a tracking system, it might be 1 standard deviation of the target's motion; in an organization, it might be the strategy-reality gap at which decisions become counterproductive. The persistence condition $\mathcal{T} > \rho/\|\delta_{\text{critical}}\|$ says: your adaptive tempo must be large enough that the steady-state mismatch $\rho/\mathcal{T}$ stays below the critical line. When $\|\delta_{\text{critical}}\|$ is normalized to 1 (measuring everything in units of the critical threshold), this simplifies to $\mathcal{T} > \rho$. This is the survival condition: tempo must exceed environmental volatility (in normalized units), or the agent's model degrades past usefulness.

---

## REDESIGN 3: THE LYAPUNOV / ADAPTIVE RESERVE (replaces current Slide 24)

### Slide 24a: The Correction Landscape — Visual

# THE CORRECTION LANDSCAPE

### Appendix A — Lyapunov Stability

The linear ODE (Slide 23) is a first approximation. Real correction dynamics are nonlinear. Lyapunov analysis handles the general case.

**The sector condition** — correction always points inward, proportional to mismatch:

$$\delta^T F(\mathcal{T}, \delta) \geq \alpha\|\delta\|^2 \quad \text{for } \|\delta\| \leq R$$

**Visualization**: A 2D "mismatch space" (visualizing a 2D mismatch vector $\delta = (\delta_1, \delta_2)$, even though the theory is general to $n$ dimensions). The origin (0,0) is the center — "perfect model, zero mismatch."

**THREE CONCENTRIC CIRCLES** from outside in:

**Outermost circle** (radius $R$, dark dashed border): "MODEL CLASS BOUNDARY — the sector condition holds inside this circle." Beyond this boundary, the correction mechanism may break down. Label: "$R$ = model class capacity."

**Middle circle** (radius $R^*$, solid orange border, filled with light orange): "ULTIMATELY BOUNDED REGION — where mismatch settles." $R^* = \rho/\alpha$. All trajectories starting inside $R$ eventually enter and stay within this circle.

**Center point** (origin): "Zero mismatch (theoretical ideal, never reached because $\rho > 0$)."

**FLOW ARROWS** throughout the diagram:
- Inside $R$: arrows all point INWARD (toward the center), spiraling into the $R^*$ ball. The arrows are longer at larger $\|\delta\|$ — stronger correction for larger errors.
- Inside the $R^*$ ball: arrows are shorter, approximately balanced by the disturbance. Trajectories meander within this ball without escaping.
- Outside $R$ (if shown): arrows become erratic, some pointing outward — correction has broken down.

**THE GAP** between $R^*$ and $R$ is visually prominent — labeled $\Delta\rho^*$ with an arrow spanning the gap. This is the adaptive reserve.

> **Notation**: The diagram moves from 1D (the number-line pictures of Slides 23a-b) to 2D mismatch space — the mismatch vector $\delta$ now has components $(\delta_1, \delta_2)$, and $\|\delta\|$ is its length (distance from the origin). $F(\mathcal{T}, \delta)$ is the general correction function — replacing the linear $\mathcal{T} \cdot \delta$ with a potentially nonlinear function. The sector condition $\delta^T F(\mathcal{T}, \delta) \geq \alpha\|\delta\|^2$ is a dot product condition: $\delta^T F$ measures whether the correction vector is "aligned with" the mismatch (pointing inward). The condition says this alignment is at least $\alpha$ times the squared mismatch magnitude, where $\alpha$ is the lower correction efficiency bound — the worst-case correction rate within the valid region. $R$ is the radius where this guarantee holds — the model class capacity. Beyond $R$, the correction mechanism may fail (connecting to TF-10: structural adaptation needed). The flow arrows directly visualize the correction: at each point $\delta$, the arrow shows which way the correction pushes.

---

### Slide 24b: Adaptive Reserve and Shock — Visual

# ADAPTIVE RESERVE

### Proposition A.2

$$\boxed{\Delta\rho^* = \alpha R - \rho}$$

**The gap between total correction capacity and current demand.**

**Visualization**: The same concentric-circle diagram from 24a, but now shown in THREE states side by side (or as an animated sequence):

**STATE 1: ROBUST** (large Δρ*)
- $R^*$ is small relative to $R$ — the inner orange ball is tiny compared to the outer boundary.
- The gap (reserve) is large. Labeled: "Lots of room. Can absorb major shocks."
- A "shock wave" icon approaches from outside — the agent can absorb it.
- Examples: "Organism in stable niche. Well-capitalized firm. Force with operational depth."

**STATE 2: FRAGILE** (small Δρ*)
- $R^*$ has grown to nearly fill $R$ — the inner ball is almost touching the outer boundary.
- The gap (reserve) is a thin sliver. Labeled: "Near capacity. Minimal shock tolerance."
- The same shock wave would push $R^*$ past $R$ — causing breakdown.
- Examples: "At culmination point. Startup in volatile market. Filter on erratic target."

**STATE 3: FAILURE** (Δρ* < 0 after shock)
- $R^*$ has expanded past $R$ — the inner ball exceeds the outer boundary.
- Arrows outside $R$ are erratic/outward — correction has broken down.
- Labeled: "Correction mechanism overwhelmed. Structural adaptation required (TF-10)."

The key visual: the reserve $\Delta\rho^*$ IS the visible gap. Robustness is literally how much space is left between where you are and where you break.

Below: a "gauge" representation of the same idea — a fuel gauge where the total capacity is $\alpha R$, the current demand is $\rho$, and the remaining fuel is $\Delta\rho^* = \alpha R - \rho$. When the gauge empties: model failure.

> **Notation**: $\Delta\rho^*$ (delta-rho-star) is the adaptive reserve — a single number characterizing the agent's robustness to sudden environmental change. $\alpha R$ is the total correction capacity: $\alpha$ (the worst-case correction efficiency) times $R$ (the radius where correction works). $\rho$ is the current demand (disturbance rate). The difference $\alpha R - \rho$ is what's left over — how much additional disturbance can be absorbed before $R^* = \rho/\alpha$ exceeds $R$ (the model class boundary), triggering correction breakdown. The three states in the visual correspond to: $\Delta\rho^* \gg 0$ (robust — operating well below capacity), $\Delta\rho^* \approx 0$ (fragile — at capacity), and $\Delta\rho^* < 0$ (failed — correction overwhelmed, structural adaptation needed per TF-10). The connection to adversarial dynamics (Prop A.3): an adversary destabilizes you by adding $\gamma_A \cdot \mathcal{T}_A$ to your $\rho$, consuming your reserve.
