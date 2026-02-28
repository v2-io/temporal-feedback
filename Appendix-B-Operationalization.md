# Appendix B: Operationalization and Worked Example

**Epistemic status**: This appendix is a procedures document. It specifies estimation recipes for core TFT quantities and gives one compact end-to-end worked example that instantiates the full chain from TF-01 through Appendix A under explicit assumptions.

## Purpose

This appendix addresses the measurement gap between TFT's formal objects and practical deployment:

1. How to estimate $U_M$, $U_o$, $\rho$, $\alpha$, $R$, and $\|\delta_{\text{critical}}\|$ from data.
2. How to run one coherent domain instantiation that touches TF-01 through Appendix A without hidden steps.

## B.1 Measurement Targets

| Quantity | Role in TFT | Typical unit | Minimum data needed |
|----------|-------------|--------------|---------------------|
| $U_M$ | Model uncertainty (TF-06) | domain-specific variance/entropy | Predictive posterior or ensemble spread |
| $U_o$ | Observation uncertainty (TF-04/TF-06) | sensor variance/noise scale | Channel calibration or residual variance |
| $\rho(t)$ | Mismatch injection rate (TF-11) | surprise per time | Time-series of mismatch magnitudes |
| $\rho_{\text{delib}}$ | Local mismatch drift during pauses (TF-09) | surprise per time | Deliberation windows with no corrective action |
| $\alpha$ | Lower correction efficiency bound (App. A) | inverse time | Vector mismatch trajectories + correction term |
| $R$ | Radius where local sector condition holds (App. A) | surprise magnitude | Same as $\alpha$, plus breakdown detection |
| $\|\delta_{\text{critical}}\|$ | Functional adequacy threshold (TF-11) | surprise magnitude | Task-level performance curve vs mismatch |

## B.2 Estimator Cookbook

### B.2.1 Estimating $U_M$ and $U_o$

Use the most native uncertainty representation available in the domain:

| Domain                   | $U_M$ estimator                                                  | $U_o$ estimator                                          |
| ------------------------ | ---------------------------------------------------------------- | -------------------------------------------------------- |
| Kalman / linear Gaussian | Prior predictive variance $P_{t \vert t-1}$                      | Measurement-noise variance $R_t$ (known or EM-estimated) |
| Conjugate Bayes          | Posterior variance / inverse effective sample size               | Likelihood variance (or precision inverse)               |
| RL with ensembles        | Across-head predictive variance $\operatorname{Var}_i[Q_i(s,a)]$ | TD-target noise variance over replay batches             |
| Neural net regression    | Ensemble or Laplace posterior variance                           | Aleatoric head output $\sigma^2(x)$                      |
| PID / classical control  | State-estimation covariance from observer                        | Sensor noise from calibration + residual PSD             |

For TFT's scalar gain heuristic (TF-06), normalize to common units and compute:

*[Operational Definition]*
$$\hat{\eta}^*_t = \frac{\hat{U}_{M,t}}{\hat{U}_{M,t} + \hat{U}_{o,t}}$$

### B.2.2 Estimating $\rho(t)$ and $\rho_{\text{delib}}$

Let $s_t = \|\delta_t\|$ in surprise units (e.g., negative log-likelihood residual scale).

Global mismatch injection rate:

*[Operational Definition]*
$$\hat{\rho}(t) = \left[\frac{s_{t+\Delta t} - s_t}{\Delta t} + \hat{\mathcal{T}}_t \, s_t\right]_+$$

where $[x]_+ = \max(x, 0)$ and $\hat{\mathcal{T}}_t$ is estimated adaptive tempo.

Local pause-window drift for TF-09:

*[Operational Definition]*
$$\hat{\rho}_{\text{delib}} = \operatorname*{median}_{w \in \mathcal{W}_{\text{pause}}} \frac{s_{w,\text{end}} - s_{w,\text{start}}}{\Delta\tau_w}$$

using windows where corrective action is suspended or effectively delayed.

### B.2.3 Estimating $\alpha$ (sector lower bound)

Appendix A uses:

*[Assumption A2']*
$$\delta^T F(\mathcal{T}, \delta) \ge \alpha \|\delta\|^2 \quad \text{for } \|\delta\| \le R$$

Operationally:

1. Estimate $\dot{\delta}_t$ (finite differences or filtered derivative).
2. Compute $\widehat{F}_t = -\dot{\delta}_t + w_t$ where disturbance proxy $w_t$ is estimated from exogenous perturbation channels or residual balancing.
3. Form ratios $r_t = (\delta_t^T \widehat{F}_t) / \|\delta_t\|^2$ on bins of $\|\delta_t\|$.
4. Set conservative lower bound $\hat{\alpha}$ as a low quantile (for example 10th percentile) of $r_t$ in the valid region.

### B.2.4 Estimating $R$ (valid-region radius)

Estimate $R$ as the largest radius for which sector inequality violations remain below tolerance:

*[Operational Criterion]*
$$\hat{R} = \sup \left\{ r > 0 : \Pr\left(\delta^T \widehat{F} < \hat{\alpha}\|\delta\|^2 \,\middle|\, \|\delta\| \le r\right) \le \epsilon \right\}$$

with a chosen violation tolerance $\epsilon$ (for example 5%).

### B.2.5 Estimating $\|\delta_{\text{critical}}\|$

Define a mission-level performance metric $J$ (reward rate, tracking error, service SLA, etc.).
Set the critical mismatch threshold where performance crosses a minimum acceptable level $J_{\min}$:

*[Operational Definition]*
$$\|\hat{\delta}_{\text{critical}}\| = \inf \left\{ d : \mathbb{E}[J \mid \|\delta\| = d] < J_{\min} \right\}$$

This anchors TF-11's normalized persistence condition to real task outcomes.

## B.3 Recommended Estimation Sequence

1. Fix mismatch representation $\delta$ in one consistent unit system (prefer surprise-scale).
2. Estimate $U_o$ from channel physics/calibration; estimate $U_M$ from model uncertainty.
3. Validate gain behavior against TF-06 ($\hat{\eta}^*$ trend checks).
4. Estimate $\rho_{\text{delib}}$ from pause windows (TF-09) and $\rho(t)$ from full traces (TF-11).
5. Estimate $\alpha$ and $R$ from local correction dynamics (Appendix A).
6. Estimate $\|\delta_{\text{critical}}\|$ from task-performance degradation.
7. Compute derived diagnostics: tempo margin $\hat{\mathcal{T}} - \hat{\rho}/\|\hat{\delta}_{\text{critical}}\|$, reserve $\widehat{\Delta \rho^*} = \hat{\alpha}\hat{R} - \hat{\rho}$, and deliberation feasibility $\Delta\eta^*(\Delta\tau)\|\delta_{\text{post}}\| - \hat{\rho}_{\text{delib}}\Delta\tau$.

## B.4 Compact End-to-End Worked Example (TF-01 -> Appendix A)

### System: 1D active tracking with selectable sensor mode

The agent tracks scalar state $x_t$ and chooses sensor mode $a_t \in \{L, H\}$:

- Dynamics: $x_{t+1} = x_t + v_t$, $v_t \sim \mathcal{N}(0, q)$, $q = 0.25$
- Observation: $y_t = x_t + n_t^{(a_t)}$
- Low mode noise: $r_L = 9$
- High mode noise: $r_H = 1$
- Event rate: $\nu = 5 \text{ Hz}$
- High mode has higher energy cost

This setup is minimal but includes both action-conditioned sensing and online model updating.

### TF-01 (scope)

- $\Omega_t = x_t$ is partially observed via noisy channel.
- $\mathcal{A} = \{L, H\}$ is non-empty and causally affects observation quality.
- Residual uncertainty persists ($H(\Omega_t \mid \mathcal{H}_t) > 0$) due to process and sensor noise.

### TF-02 (causal structure + CIY)

Action precedes observation and changes $P(y_t \mid do(a_t))$ through $r_{a_t}$.
Using low mode as comparator action ($q(a') = \mathbf{1}[a' = L]$), the canonical TF-02 CIY is the interventional KL divergence:

*[Worked Quantity]*
$$\text{CIY}(H) = D_{\mathrm{KL}}\!\big(P(y \mid do(H)) \,\|\, P(y \mid do(L))\big)$$
$$= \frac{1}{2}\left[\log\!\left(\frac{P^- + r_L}{P^- + r_H}\right) + \frac{P^- + r_H}{P^- + r_L} - 1\right]$$

With $P^- = 4.25$, this gives:
$$\text{CIY}(H) = \frac{1}{2}\left[\log\!\left(\frac{13.25}{5.25}\right) + \frac{5.25}{13.25} - 1\right] \approx 0.161 \text{ nats}$$

### TF-03 (model)

Model state $M_t = (\hat{x}_{t|t}, P_{t|t})$ is a compression of interaction history with recursive update.

### TF-04 (event-driven dynamics)

Each sensor read is an observation event; updates occur asynchronously at $\nu = 5 \text{ Hz}$.

### TF-05 (mismatch)

Innovation:

*[Worked Quantity]*
$$\delta_t = y_t - \hat{x}_{t|t-1}$$

### TF-06 (update gain)

Scalar Kalman gain:

*[Worked Quantity]*
$$K_t = \frac{P^-_t}{P^-_t + r_{a_t}}$$

With $P^- = 4.25$:

- $K(H) = 4.25 / 5.25 \approx 0.810$
- $K(L) = 4.25 / 13.25 \approx 0.321$

Exact TF-06 uncertainty ratio mapping:
- $U_M = P^-_t$
- $U_o = r_{a_t}$
- $\eta^* = K_t$

### TF-07 and TF-08 (action selection + exploration)

Use policy objective:

*[Worked Objective]*
$$a_t^* = \arg\max_a \left[\mathbb{E}[\text{value}(a)\mid M_t] + \lambda_t \, \mathbb{E}[\text{CIY}(a)\mid M_t]\right]$$

When uncertainty is high ($P^-$ large), $\lambda_t$ and the CIY term favor high mode $H$.
As uncertainty falls, policy shifts toward low-cost $L$.

### TF-09 (deliberation threshold)

Suppose a planning pause of $\Delta\tau = 0.5 \text{ s}$, with measured local pause drift:

*[Measured]*
$$\rho_{\text{delib}} = 0.40 \;\text{surprise/s}$$

Cost during pause: $\rho_{\text{delib}} \cdot \Delta\tau = 0.20$ surprise units.
If $\|\delta_{\text{post}}\| = 0.70$, deliberation is worthwhile iff:

*[Threshold]*
$$\Delta\eta^*(0.5)\cdot 0.70 > 0.20 \;\Longrightarrow\; \Delta\eta^*(0.5) > 0.286$$

If expected gain improvement is below $0.286$, act immediately.

### TF-10 (structural adaptation trigger)

Assume maneuvering regime change introduces sustained residual autocorrelation and mismatch floor.
If estimated valid radius drops to $R = 0.08$ while current bound radius is $R^* = \rho/\alpha = 0.12$, parametric adaptation is no longer adequate ($R^* > R$), triggering model-class change (for example constant-velocity -> constant-acceleration process model).

### TF-11 (tempo + persistence)

Using action mix $70\% H, 30\% L$:

*[Worked Quantity]*
$$\bar{\eta}^* = 0.7(0.810) + 0.3(0.321) = 0.663$$
*[Worked Quantity]*
$$\mathcal{T} = \nu \bar{\eta}^* = 5 \cdot 0.663 = 3.315 \;\text{s}^{-1}$$

With $\rho = 0.18 \text{ surprise/s}$ and $\|\delta_{\text{critical}}\| = 1$, persistence condition holds:

*[Check]*
$$\mathcal{T} > \frac{\rho}{\|\delta_{\text{critical}}\|} \;\;\Rightarrow\;\; 3.315 > 0.18$$

### Appendix A (Lyapunov robustness mapping)

From data, suppose conservative estimates:

- $\alpha = 2.6 \text{ s}^{-1}$
- $R = 1.4$
- $\rho = 0.18$

Then:

*[Worked Bounds]*
$$R^* = \frac{\rho}{\alpha} = \frac{0.18}{2.6} \approx 0.069 < R$$
*[Worked Reserve]*
$$\Delta\rho^* = \alpha R - \rho = 2.6(1.4) - 0.18 = 3.46$$

Interpretation:

- The agent is comfortably within its invariant region.
- It has substantial adaptive reserve to absorb additional disturbance before structural failure.

This completes one explicit path from TF-01 through Appendix A with measurable quantities at each step.

## B.5 Minimal Reproducibility Checklist

For any domain report claiming TFT validation, include:

1. Mismatch definition and units.
2. Estimation method for $U_M$, $U_o$, and uncertainty calibration diagnostics.
3. Estimation method for $\rho_{\text{delib}}$ and $\rho(t)$ with window definitions.
4. Sector-bound estimation method ($\alpha$, $R$) and violation tolerance $\epsilon$.
5. Task-level definition of $\|\delta_{\text{critical}}\|$.
6. At least one ablation where $\eta$ is intentionally miscalibrated to test TF-06 predictions.
7. At least one induced-shock test to evaluate reserve prediction $\Delta\rho^*$.
