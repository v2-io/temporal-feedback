# Working notes for Appendix F

Key structural decisions:
- Each agent is a local TFT loop. Multi-agent is a *coupling layer*, not a replacement.
- Communication gain extends TF-06's uncertainty ratio to social channels.
- Distributed tempo extends TF-11 to include comm channels.
- Team persistence extends App A with cooperative/adversarial rho decomposition.
- Game theory enters for incentive compatibility (when is honest comm rational?).
- Decision theory enters for operational calibration (lambda, stopping, switching).

Epistemic status of each piece:
- Model extension: Formulation (representational choice)
- Communication gain: Hypothesis (extending uncertainty ratio)
- Distributed tempo: Definition (straightforward sum)
- Team persistence: Derived (from model + App A sector condition, conditional on communication gain hypothesis)
- Topology analysis: Discussion (qualitative consequences)
- Game-theoretic integration: Discussion (identifies where existing GT results apply)

Key insight to highlight: TFT provides the formal objects (mismatch, gain, tempo, reserve) that game theory can *operate on*. TFT doesn't replace game theory; it gives GT the right state variables.

Novel contribution: The cooperative term in rho is negative — allies reduce your mismatch. This means teams can persist where individuals cannot. The formalism shows *exactly* how much cooperation is needed: gamma_coop * T_ally must offset the gap between T_i and rho_i/R_i.

Coordination overhead: Adding communication channels increases T_i but also costs time/compute. There's a diminishing-returns curve — eventually adding more allies costs more in coordination than it adds in tempo. This should be derivable from the distributed tempo formula.

Open question: emergence. When does the network exhibit qualitatively different behavior than any individual agent? TFT gives a partial answer: when the network's effective tempo exceeds any individual's, the network can persist in environments where no individual can. But "emergence" in the richer sense (novel model classes arising from interaction) requires structural adaptation (TF-10) at the network level — an area where TFT currently has nothing formal.
