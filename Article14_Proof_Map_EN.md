# Paper 14: manuscript-to-Lean map

Constrained Setpoint-Zero Shaping in Two-Degree-of-Freedom PIDF Control: Modal Suppression, Monotone Tracking, and MIACE with Pure Delay

**English map dated 24 October 2026 · Formalization v0.4.1**

18 numbered results · 140 distinct directly mapped declarations (161 occurrences) · 62 Article14 modules · 779 theorems · 1005 named declarations.

The map preserves the 18 recorded complete correspondences at formalization version 0.4.1. Lean checks formal declarations; correspondence between prose and formal statements is a separate semantic assessment. Read the full hypotheses in both the paper and the Lean theorem types.

For statement/proof text beside Lean source, download and open [Article14_Proof_Map_EN.html](Article14_Proof_Map_EN.html). The machine-readable version is [proof-map.en.json](docs/article14/proof-map.en.json).

## Reading map

| Result | Title | Paper label | Mapped declarations |
|---|---|---|---|
| Proposition 1 | [Separation of the fixed feedback channels](#result-1) | `prop:separation` | 2 |
| Theorem 2 | [Exact constrained assignment](#result-2) | `thm:feasibility` | 4 |
| Theorem 3 | [Cross-placement command-jump identity](#result-3) | `thm:jump` | 3 |
| Corollary 4 | [Exact target under a jump limit](#result-4) | `cor:jump-feasible` | 2 |
| Corollary 5 | [A necessary damping and frequency bound](#result-5) | `cor:damping-floor` | 4 |
| Theorem 6 | [Exact reference-channel cancellation](#result-6) | `thm:cancellation` | 2 |
| Theorem 7 | [Tight disk mismatch bound](#result-7) | `thm:disk-bound` | 1 |
| Corollary 8 | [Uniform modal-residue bound](#result-8) | `cor:residue-bound` | 2 |
| Corollary 9 | [Step-mode mismatch bound](#result-9) | `cor:step-mismatch` | 3 |
| Proposition 10 | [Finite-scenario convexity and existence](#result-10) | `prop:socp` | 27 |
| Proposition 11 | [A command limit throughout a time interval](#result-11) | `prop:time-tightening` | 1 |
| Proposition 12 | [A sufficient family-wide feedback condition](#result-12) | `prop:continuum-stability` | 11 |
| Proposition 13 | [Exact preservation of the feedback channels](#result-13) | `prop:delay-hinf` | 8 |
| Theorem 14 | [Fastest mean response with a fixed delayed feedback loop](#result-14) | `thm:delay-fastest` | 31 |
| Proposition 15 | [Second moment and a secondary choice of c](#result-15) | `prop:delay-second-moment` | 15 |
| Proposition 16 | [Exact settling-time feasibility](#result-16) | `prop:delay-settling` | 13 |
| Theorem 17 | [MIACE reduction within the fixed-feedback family](#result-17) | `thm:delay-miace` | 19 |
| Theorem 18 | [A continuum of exact MIACE optima using only b,c](#result-18) | `thm:exact-delay-family` | 13 |

## Scope and assumptions

- The 18-result count covers the numbered statements and their documented interfaces. It does not cover every unnumbered discussion, a family-wide uniform spectral margin, or numerical computations.
- The unchanged fixed disturbance/noise channels and the unit frequency norm of a positive unit-gain reference channel are separate conclusions. The formal kernelHinf is the supremum of the modulus of the physical kernel's complex Laplace transform on the imaginary axis; no separate abstract Hardy-space development is claimed.
- The general delayed-plant results assume internal exponential stability and require the stated nonempty feasible sets. They do not assert feasible monotone tuning for every plant, or identify mean response time with settling time.
- Theorem 18 compares MIACE with the stated integrable plant-response class. Its whole-box IAE conclusion is for the specified exact example, including nonmonotone competitors.
- Shared MIACE results are inherited from Paper 13 and are not counted as new numbered results of Paper 14. The allowed logical axioms are propext, Classical.choice, and Quot.sound. This map adds no mathematical axiom or Lean declaration.

The 18 statement blocks and 17 explicit proof blocks in the current MCSS manuscript match the supplied v0.4.1 paper sources after comments and whitespace are removed. Corollary 4 has no separate proof block. Proof-route differences below explain how the formal arguments implement the mathematical reasoning.

## Verification

Use the pinned Lean/mathlib versions and follow the package README and validation report. Basic commands from the project directory:

```bash
lake exe cache get
lake build Article14
lake env lean AuditArticle14.lean
```

This map adds no Lean declaration and is not a new compilation record. A local build and a live.lean browser run are separate checks.

<a id="result-1"></a>

## Proposition 1: Separation of the fixed feedback channels

**Paper label:** `prop:separation`  
**Paper source:** `sections/02_architecture.tex`  
**Mapping status:** Complete, with the hypotheses below.

The homogeneous equations and all six external channels are derived from the shared-state realization.

### Hypotheses

- Strictly proper real-rational minimal plant
- Positive Kp, Ti, Td, Tf
- Shared integral and derivative-filter states as explicitly implemented

### Mathematical obligations

- Derive the homogeneous closed-loop state equations and show independence of b,c
- Derive disturbance and noise transfers from interconnected equations
- Identify y/r=P Cr S and u/r=Cr S

**Completion criterion:** Defining S,T,PS,CyS without b,c and proving reflexive equality establishes transfer-expression invariance but does not establish the physical shared-state realization claim.

### Exact Lean declarations

- [`Article14.sharedClosedLoop_homogeneous_independent`](Article14/PIDFAlgebra.lean#L41) — `Article14/PIDFAlgebra.lean`, line 41.
- [`Article14.closedLoopTransfers_correct`](Article14/PIDFAlgebra.lean#L200) — `Article14/PIDFAlgebra.lean`, line 200.

<a id="result-2"></a>

## Theorem 2: Exact constrained assignment

**Paper label:** `thm:feasibility`  
**Paper source:** `sections/03_geometry.tex`  
**Mapping status:** Complete, with the hypotheses below.

Equality of the actual polynomials, unique inverse formulas, and feasibility are proved for both filter placements.

### Hypotheses

- Ti,Td,Tf>0
- Finite lower and upper bounds on b,c
- Target Q=1+sigma*s+pi*s^2

### Mathematical obligations

- Prove polynomial equality equivalent to the two coefficient equations
- Prove unique filtered-D inverse b=(sigma-Tf)/Ti and c=(pi-Tf*(sigma-Tf))/(Ti*Td)
- Prove unique output-filter inverse b=sigma/Ti and c=pi/(Ti*Td)
- Prove both box-feasibility iff statements
- Represent the feasible coefficient sets as the affine image parallelogram and diagonal image rectangle, including degenerate boxes if permitted

**Completion criterion:** Inverse substitution alone omits uniqueness, both feasibility directions, and set geometry.

### Exact Lean declarations

- [`Article14.dPolynomial_assignment_iff`](Article14/PIDFAlgebra.lean#L314) — `Article14/PIDFAlgebra.lean`, line 314.
- [`Article14.oPolynomial_assignment_iff`](Article14/PIDFAlgebra.lean#L354) — `Article14/PIDFAlgebra.lean`, line 354.
- [`Article14.dAssignment_exists_iff`](Article14/PIDFAlgebra.lean#L389) — `Article14/PIDFAlgebra.lean`, line 389.
- [`Article14.oAssignment_exists_iff`](Article14/PIDFAlgebra.lean#L408) — `Article14/PIDFAlgebra.lean`, line 408.

<a id="result-3"></a>

## Theorem 3: Cross-placement command-jump identity

**Paper label:** `thm:jump`  
**Paper source:** `sections/03_geometry.tex`  
**Mapping status:** Complete, with the hypotheses below.

The actual command right limits are proved for both filter placements, for a given continuous zero-state response and a strictly proper output y = Cx.

### Hypotheses

- Strictly proper plant
- Zero initial states
- Unsmoothed reference step Delta_r
- No saturation or rate limiting
- Positive gains and filter times

### Mathematical obligations

- Derive actual right-hand command values Kp*(b+c*Td/Tf)*Delta_r and Kp*c*Td/Tf*Delta_r
- Substitute the matched coefficients and prove the common value Kp*pi*Delta_r/(Ti*Tf)

**Dependencies:** [thm:feasibility](#result-2).

**Reuse from Paper 13:** `MIACE.PIDTimeStates`, `MIACE.OrdinaryPID`.

**Proof route:** Continuity of state-output realizations proves the actual command right limit; no transfer initial-value theorem is assumed.

**Completion criterion:** Substitution of the inverse weights proves equality of the candidate jump formulas; a physical initial-value or state-feedthrough bridge is still required.

### Exact Lean declarations

- [`Article14.dAssignedStepCommand_rightLimit`](Article14/PIDFAlgebra.lean#L554) — `Article14/PIDFAlgebra.lean`, line 554.
- [`Article14.oAssignedStepCommand_rightLimit`](Article14/PIDFAlgebra.lean#L568) — `Article14/PIDFAlgebra.lean`, line 568.
- [`Article14.outputFilteredController_laplace_law`](Article14/PIDFAlgebra.lean#L452) — `Article14/PIDFAlgebra.lean`, line 452.

<a id="result-4"></a>

## Corollary 4: Exact target under a jump limit

**Paper label:** `cor:jump-feasible`  
**Paper source:** `sections/03_geometry.tex`  
**Mapping status:** Complete, with the hypotheses below.

The command bound is characterized by an exact equivalence for the actual command right limit.

### Hypotheses

- Matched target with pi>0
- Nonzero reference step
- Absolute initial command bound
- Positive Kp,Ti,Tf

### Mathematical obligations

- Derive pi<=u_max*Ti*Tf/(Kp*abs(Delta_r)) from the actual jump identity

**Dependencies:** [thm:jump](#result-3).

**Completion criterion:** Conditional corollary from an assumed jump formula is an algebraic result until theorem 3's physical jump bridge is complete.

### Exact Lean declarations

- [`Article14.dAssignedStepCommand_jump_bound_iff`](Article14/PIDFAlgebra.lean#L606) — `Article14/PIDFAlgebra.lean`, line 606.
- [`Article14.oAssignedStepCommand_jump_bound_iff`](Article14/PIDFAlgebra.lean#L626) — `Article14/PIDFAlgebra.lean`, line 626.

No separate proof block is printed for this corollary; it follows directly from the preceding command-jump identity (Theorem 3).

<a id="result-5"></a>

## Corollary 5: A necessary damping and frequency bound

**Paper label:** `cor:damping-floor`  
**Paper source:** `sections/03_geometry.tex`  
**Mapping status:** Complete, with the hypotheses below.

Both frequency bounds, the square-root damping bound, and an explicit attaining choice are proved.

### Hypotheses

- omega_z>0
- 0<zeta_z<1
- Filtered-D exact matching
- b>=0
- u_max>0 and nonzero reference step

### Mathematical obligations

- Derive the lower frequency bound from the jump limit
- Derive omega_z<=2*zeta_z/Tf from b>=0
- Derive the square-root damping lower bound
- Construct sharpness witness b=0,c=1/(Ti*Td*omega_z^2) when the damping lower bound is below one
- State sharpness only without upper weight constraints

**Dependencies:** [thm:feasibility](#result-2), [cor:jump-feasible](#result-4).

**Completion criterion:** Squared inequalities are sufficient only after the nonnegative square-root branches and the explicit sharpness witness are proved.

### Exact Lean declarations

- [`Article14.dAssignedStepCommand_damping_floor`](Article14/PIDFAlgebra.lean#L701) — `Article14/PIDFAlgebra.lean`, line 701.
- [`Article14.damping_floor_sharp_witness`](Article14/PIDFAlgebra.lean#L728) — `Article14/PIDFAlgebra.lean`, line 728.
- [`Article14.sharp_witness_damping_eq_floor`](Article14/PIDFAlgebra.lean#L753) — `Article14/PIDFAlgebra.lean`, line 753.
- [`Article14.sharp_witness_complex`](Article14/PIDFAlgebra.lean#L766) — `Article14/PIDFAlgebra.lean`, line 766.

<a id="result-6"></a>

## Theorem 6: Exact reference-channel cancellation

**Paper label:** `thm:cancellation`  
**Paper source:** `sections/04_cancellation_bounds.tex`  
**Mapping status:** Complete, with the hypotheses below.

The selected complex roots are absent from the canonical reduced denominator exactly when the normalized quadratic matching condition holds.

### Hypotheses

- Delta=q*h, q=s^2+a1*s+a0
- a0>0
- q coprime to Kp*B*h
- q has a stable nonreal conjugate pair
- ND has degree at most two and constant coefficient one

### Mathematical obligations

- Formalize disappearance of both roots from the reduced reference transfer
- Use coprimality and simple conjugate roots to show disappearance iff q divides ND
- Prove divisibility iff ND=q/a0
- Transport theorem 2's feasibility characterization

**Dependencies:** [thm:feasibility](#result-2).

**Reuse from Paper 13:** `MIACE.HermiteUniqueness`, `MIACE.RationalUniqueness`.

**Proof route:** The proof uses canonical reduced denominators of rational functions and the complex-root criterion over an algebraically closed field.

**Completion criterion:** Matching coefficients or assuming polynomial divisibility omits the rational-transfer cancellation equivalence.

### Exact Lean declarations

- [`Article14.paper_exact_reference_cancellation`](Article14/Cancellation.lean#L151) — `Article14/Cancellation.lean`, line 151.
- [`Article14.paper_exact_cancellation_feasibility`](Article14/Cancellation.lean#L164) — `Article14/Cancellation.lean`, line 164.

<a id="result-7"></a>

## Theorem 7: Tight disk mismatch bound

**Paper label:** `thm:disk-bound`  
**Paper source:** `sections/04_cancellation_bounds.tex`  
**Mapping status:** Complete, with the hypotheses below.

The complex identity, modulus bound, and an attaining displacement are proved.

### Hypotheses

- Nonzero nominal pole p_hat inherited from the stable selected-mode convention
- epsilon>=0
- p=p_hat+e and norm(e)<=epsilon

### Mathematical obligations

- Prove q_hat(p)=e*(2*i*im(p_hat)+e)/norm(p_hat)^2
- Prove the norm upper bound
- Exhibit a displacement attaining the bound on the unrestricted disk, including im(p_hat)=0 and epsilon=0 cases

**Completion criterion:** The residual polynomial identity alone does not prove the norm estimate or its sharpness.

### Exact Lean declarations

- [`Article14.paper_tight_disk_mismatch`](Article14/ModalBounds.lean#L94) — `Article14/ModalBounds.lean`, line 94.

<a id="result-8"></a>

## Corollary 8: Uniform modal-residue bound

**Paper label:** `cor:residue-bound`  
**Paper source:** `sections/04_cancellation_bounds.tex`  
**Mapping status:** Complete, with the hypotheses below.

The residue is an actual local limit derived from the derivative of the denominator.

### Hypotheses

- Compact scenario set
- Stable simple selected poles separated from other poles
- Uniform bound norm(H(p)/D'(p))<=M
- Pole displacement <=epsilon
- Nominal numerator matching

### Mathematical obligations

- Derive the analytic simple-pole residue formula
- Apply theorem 7 and the prefactor bound uniformly

**Dependencies:** [thm:disk-bound](#result-7).

**Completion criterion:** A product norm bound for a quantity named residue is conditional until the limit/residue characterization is supplied.

### Exact Lean declarations

- [`Article14.simple_residue_of_derivative`](Article14/ModalBounds.lean#L117) — `Article14/ModalBounds.lean`, line 117.
- [`Article14.paper_uniform_residue_bound`](Article14/ModalBounds.lean#L175) — `Article14/ModalBounds.lean`, line 175.

<a id="result-9"></a>

## Corollary 9: Step-mode mismatch bound

**Paper label:** `cor:step-mismatch`  
**Paper source:** `sections/04b_step_transients.tex`  
**Mapping status:** Complete, with the hypotheses below.

The step residue r/p, actual Laplace integrals, and the exponential bound for the conjugate pair are proved.

### Hypotheses

- Theorem 8 assumptions
- norm(p)>=m_p>0
- -re(p)>=alpha_star>0
- t>=0
- Real transfer and conjugate poles/residues

### Mathematical obligations

- Derive step-mode amplitude a_p=residue/p
- Derive the real conjugate-pair expression and its envelope
- Combine the residue bound, pole-modulus lower bound and damping lower bound

**Dependencies:** [cor:residue-bound](#result-8).

**Completion criterion:** Bounding a predefined exponential pair does not identify it as the actual step transient mode.

### Exact Lean declarations

- [`Article14.simple_step_residue`](Article14/ModalBounds.lean#L135) — `Article14/ModalBounds.lean`, line 135.
- [`Article14.conjugatePair_laplace`](Article14/ModalBounds.lean#L226) — `Article14/ModalBounds.lean`, line 226.
- [`Article14.paper_step_mismatch`](Article14/ModalBounds.lean#L279) — `Article14/ModalBounds.lean`, line 279.

<a id="result-10"></a>

## Proposition 10: Finite-scenario convexity and existence

**Paper label:** `prop:socp`  
**Paper source:** `sections/05_robust_design.tex`  
**Mapping status:** Complete, with the hypotheses below.

The actual resolvent and modal residues are derived from the same A, B, C that determine the physical time-domain constraints. Step responses are constructed without DC or stability assumptions, and uniqueness is proved. For finitely many scenarios and sample times, the construction gives five explicit polygon inequalities, four inequalities per sample, and the actual modal cones. Exact epigraph equivalence and attainment of the global optimum are proved when the feasible set is nonempty.

### Hypotheses

- Finite nonempty scenario collection
- Selected stable simple nonzero poles
- Compact admitted weight polygon X
- X nonempty
- Optional fixed-time affine constraints leave a nonempty set

### Mathematical obligations

- Derive affine complex residue and step amplitudes in b,c
- Prove equivalence of the epigraph problem to finitely many second-order cones
- Prove convexity and continuity of the finite maximum
- Prove optimizer existence on the compact feasible set
- Derive affine dependence of actual fixed-time response and command from the fixed-state realization

**Dependencies:** [prop:separation](#result-1).

**Proof route:** The full shared-loop matrix and its adjugate yield the transfer and local residues, including coincidence with an open-loop pole. An additional constant coordinate converts affine forcing to a homogeneous equation and constructs responses through a matrix exponential. Uniqueness identifies each physical trajectory with the three basis responses. The finite linear inequalities and cones are explicit; compactness gives attainment of the minimum.

**Completion criterion:** For the rational zero-delay modal setting, actual finite-state step responses are constructed without DC assumptions, and their uniqueness identifies every physical response with the affine family. Selected modal residues derive from the same full state resolvent. The final finite SOCP exposes the polygon and sampled affine constraints explicitly and proves attainment under nonempty feasibility; arbitrary compact convex sets are not silently called SOCP representable.

### Exact Lean declarations

- [`Article14.finiteModalCost_le_iff`](Article14/ModalOptimization.lean#L25) — `Article14/ModalOptimization.lean`, line 25.
- [`Article14.finiteModalCost_convex`](Article14/ModalOptimization.lean#L38) — `Article14/ModalOptimization.lean`, line 38.
- [`Article14.finiteModalCost_minimizer`](Article14/ModalOptimization.lean#L54) — `Article14/ModalOptimization.lean`, line 54.
- [`Article14.constrained_modal_minimizer`](Article14/ModalOptimization.lean#L61) — `Article14/ModalOptimization.lean`, line 61.
- [`Article14.pidfReferenceValue_affine`](Article14/PhysicalModalOptimization.lean#L36) — `Article14/PhysicalModalOptimization.lean`, line 36.
- [`Article14.pidf_modal_residue_and_step_amplitude`](Article14/PhysicalModalOptimization.lean#L75) — `Article14/PhysicalModalOptimization.lean`, line 75.
- [`Article14.admittedModalPolygon_compact`](Article14/PhysicalModalOptimization.lean#L95) — `Article14/PhysicalModalOptimization.lean`, line 95.
- [`Article14.admittedModalPolygon_convex`](Article14/PhysicalModalOptimization.lean#L101) — `Article14/PhysicalModalOptimization.lean`, line 101.
- [`Article14.actual_step_samples_affine`](Article14/PhysicalModalOptimization.lean#L178) — `Article14/PhysicalModalOptimization.lean`, line 178.
- [`Article14.physicalSampleBounds_eq`](Article14/PhysicalModalOptimization.lean#L218) — `Article14/PhysicalModalOptimization.lean`, line 218.
- [`Article14.physicalSampleBounds_closed`](Article14/PhysicalModalOptimization.lean#L228) — `Article14/PhysicalModalOptimization.lean`, line 228.
- [`Article14.physicalSampleBounds_convex`](Article14/PhysicalModalOptimization.lean#L234) — `Article14/PhysicalModalOptimization.lean`, line 234.
- [`Article14.physical_modal_cone_epigraph`](Article14/PhysicalModalOptimization.lean#L249) — `Article14/PhysicalModalOptimization.lean`, line 249.
- [`Article14.physical_modal_cost_convex`](Article14/PhysicalModalOptimization.lean#L347) — `Article14/PhysicalModalOptimization.lean`, line 347.
- [`Article14.physical_modal_cost_continuous`](Article14/PhysicalModalOptimization.lean#L353) — `Article14/PhysicalModalOptimization.lean`, line 353.
- [`Article14.physical_modal_design_minimizer`](Article14/PhysicalModalOptimization.lean#L295) — `Article14/PhysicalModalOptimization.lean`, line 295.
- [`Article14.physical_modal_epigraph_minimizer`](Article14/PhysicalModalOptimization.lean#L326) — `Article14/PhysicalModalOptimization.lean`, line 326.
- [`Article14.physical_modal_minimizer_on_compact_polygon`](Article14/PhysicalModalOptimization.lean#L361) — `Article14/PhysicalModalOptimization.lean`, line 361.
- [`Article14.ModalRealization.encoded_actual_step_ode`](Article14/ModalRealizationCompatibility.lean#L388) — `Article14/ModalRealizationCompatibility.lean`, line 388.
- [`Article14.ModalRealization.referenceResolvent_eq`](Article14/ModalRealizationCompatibility.lean#L265) — `Article14/ModalRealizationCompatibility.lean`, line 265.
- [`Article14.ModalRealization.same_realization_modal_residues`](Article14/ModalRealizationCompatibility.lean#L343) — `Article14/ModalRealizationCompatibility.lean`, line 343.
- [`Article14.ModalRealization.zeroDelayStepResponse_dynamics`](Article14/ModalStepExistence.lean#L129) — `Article14/ModalStepExistence.lean`, line 129.
- [`Article14.ModalRealization.zeroDelay_step_unique`](Article14/ModalStepExistence.lean#L185) — `Article14/ModalStepExistence.lean`, line 185.
- [`Article14.ModalRealization.zeroDelay_step_eq_affine_family`](Article14/ModalStepExistence.lean#L215) — `Article14/ModalStepExistence.lean`, line 215.
- [`Article14.ModalRealization.realModalCoordinates`](Article14/ModalSOCPCompletion.lean#L82) — `Article14/ModalSOCPCompletion.lean`, line 82.
- [`Article14.ModalRealization.modalFiniteSOC_iff`](Article14/ModalSOCPCompletion.lean#L220) — `Article14/ModalSOCPCompletion.lean`, line 220.
- [`Article14.ModalRealization.modal_finite_soc_global_optimizer`](Article14/ModalSOCPCompletion.lean#L244) — `Article14/ModalSOCPCompletion.lean`, line 244.

<a id="result-11"></a>

## Proposition 11: A command limit throughout a time interval

**Paper label:** `prop:time-tightening`  
**Paper source:** `sections/05b_continuous_command.tex`  
**Mapping status:** Complete, with the hypotheses below.

An actual derivative bound and the two tightened endpoint values imply the command bound at every point of the interval.

### Hypotheses

- Ordered finite interval [a,b]
- Ordinary command with derivative bound L>=0 throughout the interval
- Endpoint magnitudes <=u_max-(b-a)*L/2
- Right-hand command value used at time zero

### Mathematical obligations

- Derive an L-Lipschitz bound from the stated differentiability/integrability hypothesis
- Use the nearest endpoint at distance <=(b-a)/2
- Prove abs(u(t))<=u_max for all t in the interval

**Proof route:** The proof uses mathlib's mean-value inequality with within derivatives at the interval endpoints.

**Completion criterion:** An endpoint result assuming LipschitzOnWith is complete for that strengthened interface; mapping the manuscript derivative hypothesis requires the calculus lemma.

### Exact Lean declarations

- [`Article14.command_bound_throughout_interval`](Article14/CommandBounds.lean#L37) — `Article14/CommandBounds.lean`, line 37.

<a id="result-12"></a>

## Proposition 12: A sufficient family-wide feedback condition

**Paper label:** `prop:continuum-stability`  
**Paper source:** `sections/06a_feedback_stability.tex`  
**Mapping status:** Complete, with the hypotheses below.

The full four-dimensional shared-PIDF matrix and its actual characteristic polynomial are derived. The paper's condition yields a negative spectrum. The exponential estimate, existence, and uniqueness are then proved for every initial state, without assuming stability or a semigroup bound in advance.

### Hypotheses

- Positive T,k,f,i,d
- zeta>=0
- f<=i
- C=d-f^2/i>0
- k>=k_min>0
- T<=T_max
- T_max^2<i*C*(1+k_min*(1+f/i))

### Mathematical obligations

- Derive the quartic characteristic polynomial from the implemented shared-state loop
- Verify positive coefficients
- Prove both displayed Hurwitz determinant expansions
- Prove H2>0 and H3>0 uniformly
- Apply/prove actual quartic Routh-Hurwitz theorem to obtain all roots in the open left half-plane
- Connect that spectrum to exponential stability of the complete implemented finite-dimensional state system

**Dependencies:** [prop:separation](#result-1).

**Reuse from Paper 13:** `MIACE.Stability`, `MIACE.PIDStateSpectrum`.

**Proof route:** The full matrix is derived from the physical equations. After algebraically excluding roots in the closed right half-plane, a sufficiently small Euler step and Gelfand's formula give the matrix exponential estimate, including repeated and nonnormal modes. Exact complexification transfers the estimate to the real system. Existence and uniqueness of all trajectories are derived.

**Completion criterion:** The explicit four-state matrix matches the implemented shared loop. Negative spectrum implies an actual real operator exponential estimate; linear ODE existence and uniqueness then prove the full internal stability predicate, without an assumed decay bound.

### Exact Lean declarations

- [`Article14.FamilyStability.family_quartic_root_re_neg`](Article14/FamilyStability.lean#L216) — `Article14/FamilyStability.lean`, line 216.
- [`Article14.FamilyStability.characteristic_expression`](Article14/FamilyStability.lean#L229) — `Article14/FamilyStability.lean`, line 229.
- [`Article14.FamilyStability.benchmark_family_root_re_neg`](Article14/FamilyStability.lean#L244) — `Article14/FamilyStability.lean`, line 244.
- [`Article14.FamilyStability.benchmark_exact_margin`](Article14/FamilyStability.lean#L112) — `Article14/FamilyStability.lean`, line 112.
- [`Article14.FamilyStability.family_state_equations`](Article14/FamilyStateMatrix.lean#L70) — `Article14/FamilyStateMatrix.lean`, line 70.
- [`Article14.FamilyStability.family_state_determinant`](Article14/FamilyStateMatrix.lean#L110) — `Article14/FamilyStateMatrix.lean`, line 110.
- [`Article14.FamilyStability.family_state_mem_spectrum_re_neg`](Article14/FamilyStateMatrix.lean#L154) — `Article14/FamilyStateMatrix.lean`, line 154.
- [`Article14.FiniteStateStability.exp_decay_of_finite_negative_spectrum`](Article14/FiniteStateStability.lean#L163) — `Article14/FiniteStateStability.lean`, line 163.
- [`Article14.realMatrixOperator_exp_norm`](Article14/MatrixComplexification.lean#L73) — `Article14/MatrixComplexification.lean`, line 73.
- [`Article14.FamilyStability.family_shared_internal_stability`](Article14/FamilyInternalStability.lean#L45) — `Article14/FamilyInternalStability.lean`, line 45.
- [`Article14.FamilyStability.benchmark_shared_internal_stability`](Article14/FamilyInternalStability.lean#L64) — `Article14/FamilyInternalStability.lean`, line 64.

<a id="result-13"></a>

## Proposition 13: Exact preservation of the feedback channels

**Paper label:** `prop:delay-hinf`  
**Paper source:** `sections/08a_monotone_delay_miace.tex`  
**Mapping status:** Complete, with the hypotheses below.

The actual delayed equations, equality of solution sets for every history, preservation of well-posedness and stability, and invariance of all disturbance-channel functionals are proved. Exact centering of the full realization and global exponential decay of a given step response follow from the same internal stability assumption.

### Hypotheses

- P_L=exp(-L*s)*P0 with L>=0 and strictly proper real-rational P0
- Positive fixed PIDF gains
- Shared-state implementation
- Actual retarded feedback loop internally exponentially stable for arbitrary admissible histories

### Mathematical obligations

- Prove homogeneous retarded/history operator independent of b,c
- Derive identical S_L,T_L,P_L*S_L,C_y*S_L and disturbance/noise matrix
- Define the relevant H-infinity norm and transport transfer/operator equality to norm equality
- Transport equality through fixed stable weighted interconnections

**Dependencies:** [prop:separation](#result-1).

**Reuse from Paper 13:** `MIACE.FiniteRetarded`, `MIACE.RetardedUniqueness`, `MIACE.IMCRobustIdentity`.

**Proof route:** The equality for all histories is applied to a centered trajectory shifted by L. Compactness of the initial interval [0,L] and internal stability give global decay; no derivative is required at the delayed jump itself.

**Completion criterion:** Equality of pointwise scalar transfer expressions establishes their norm equality once a norm is defined; it does not establish equality of the realized history operators.

### Exact Lean declarations

- [`Article14.retardedShared_solutionSet_eq`](Article14/PIDFAlgebra.lean#L100) — `Article14/PIDFAlgebra.lean`, line 100.
- [`Article14.retardedShared_wellPosed_iff`](Article14/PIDFAlgebra.lean#L144) — `Article14/PIDFAlgebra.lean`, line 144.
- [`Article14.retardedShared_exponentialStability_iff`](Article14/PIDFAlgebra.lean#L164) — `Article14/PIDFAlgebra.lean`, line 164.
- [`Article14.closedLoopTransfers_feedback_functional_independent`](Article14/PIDFAlgebra.lean#L220) — `Article14/PIDFAlgebra.lean`, line 220.
- [`Article14.retardedShared_equilibrium_translation`](Article14/PhysicalCentering.lean#L44) — `Article14/PhysicalCentering.lean`, line 44.
- [`Article14.retardedShared_stable_tail_decay`](Article14/PhysicalStability.lean#L50) — `Article14/PhysicalStability.lean`, line 50.
- [`Article14.stable_actual_step_decay`](Article14/PhysicalStability.lean#L115) — `Article14/PhysicalStability.lean`, line 115.
- [`Article14.centeredStep_after_delay`](Article14/PhysicalStep.lean#L157) — `Article14/PhysicalStep.lean`, line 157.

<a id="result-14"></a>

## Theorem 14: Fastest mean response with a fixed delayed feedback loop

**Paper label:** `thm:delay-fastest`  
**Paper source:** `sections/08a_monotone_delay_miace.tex`  
**Mapping status:** Complete, with the hypotheses below.

The entire physical family is constructed from unique forced responses of a finite-dimensional realization. The ordinary causal kernel, y = integral(g), exponential integrability, unit mass, equivalence of monotonicity and positivity, unit frequency norm, and the exact IAE/first moment are proved. The two L1 directions are independent by their first two moments. Compactness and maximization of b apply to the physical feasible set F; the strict bound m > L follows from the actual kernel support. The robust specialization permits any nonempty indexed family with a common controller and finite worst-case offset. For any given minimal realization of the reduced rational plant, DC nonsingularity, equilibrium, and the exact value of Gamma are derived.

### Hypotheses

- Theorem 13 plant and fixed-loop hypotheses
- P0(0)=k>0 and no pole at zero
- Zero initial states and zero prehistory
- Closed convex engineering set X, possibly unbounded
- Ordinary integrable impulse kernels induced by the specified PIDF plant

### Mathematical obligations

- Construct actual causal kernels g0,gb,gc and prove g_bc=g0+b*gb+c*gc
- Derive unit DC gain and total impulse mass one
- Prove a.e. impulse positivity iff nondecreasing unit-step response, with 0 and 1 endpoints
- Define/reference the frequency supremum H-infinity norm and prove it equals one
- Prove IAE=first impulse moment by Tonelli
- Derive the first moment Ti*(1-b+1/(k*Kp)) from the actual transfer/DC derivative
- Prove positivity set closed and convex in parameter space
- Prove gb,gc linearly independent from their actual transfer functions
- Use finite-dimensional norm equivalence plus the unit-mass bound to prove boundedness and hence compactness even for unbounded X
- Use compactness and Ti>0 to prove existence and equivalence to maximizing b

**Dependencies:** [prop:delay-hinf](#result-13).

**Reuse from Paper 13:** `MIACE.ComplexKernel: MIACE.complex_laplace_norm_le`, `MIACE.Causal`, `MIACE.CausalConvolution`, `MIACE.KernelTransfer`, `MIACE.Transfer: MIACE.laplace_tendsto_zero`, `MIACE.CascadeOutput: MIACE.resolvent_primitive_bounds`.

**Proof route:** The forced response is constructed from a shifted homogeneous trajectory and a proved initial transport interval. The fundamental theorem of calculus and Gronwall's inequality give uniqueness. The impulse is derived from the actual state equation. The area follows from the integrator balance, and moments follow by integration by parts with vanishing boundary terms. Independence of the two L1 directions follows from their first two moments instead of division of transfer functions. The norm is the supremum of the actual kernel's complex transform on the imaginary axis. Minimality is proved by divisibility of the characteristic polynomial by the reduced denominator. Equality of the minimal dimensions removes all additional factors and derives the DC equilibrium of the same realization.

**Completion criterion:** The minimal-realization constructor derives every static StablePIDFData obligation from the reduced strictly proper rational plant and any of its actual minimal finite realizations; only the article-assumed internal stability is an input. The listed physical-family theorems instantiate the numbered conclusion on that same realization.

### Exact Lean declarations

- [`Article14.RationalRealization.RationalPlant.companion_isMinimalRealization`](Article14/MinimalRealizationFamily.lean#L64) — `Article14/MinimalRealizationFamily.lean`, line 64.
- [`Article14.RationalRealization.minimal_dimension_eq`](Article14/MinimalRealizationFamily.lean#L57) — `Article14/MinimalRealizationFamily.lean`, line 57.
- [`Article14.RationalRealization.minimal_pencil_nonsingular_zero`](Article14/MinimalRealizationFamily.lean#L42) — `Article14/MinimalRealizationFamily.lean`, line 42.
- [`Article14.RationalRealization.minimalStableFamily`](Article14/MinimalRealizationFamily.lean#L104) — `Article14/MinimalRealizationFamily.lean`, line 104.
- [`Article14.RationalRealization.minimalStableFamily_gamma`](Article14/MinimalRealizationFamily.lean#L157) — `Article14/MinimalRealizationFamily.lean`, line 157.
- [`Article14.RationalRealization.minimal_normalized_transfer`](Article14/MinimalRealizationFamily.lean#L177) — `Article14/MinimalRealizationFamily.lean`, line 177.
- [`Article14.exists_stepLoopDynamics`](Article14/StepExistence.lean#L242) — `Article14/StepExistence.lean`, line 242.
- [`Article14.stepLoopDynamics_unique`](Article14/StepUniqueness.lean#L216) — `Article14/StepUniqueness.lean`, line 216.
- [`Article14.stepLoopDynamics_impulse_affine`](Article14/StepAffineImpulse.lean#L57) — `Article14/StepAffineImpulse.lean`, line 57.
- [`Article14.StablePIDFData.response_exists`](Article14/PhysicalFamily.lean#L53) — `Article14/PhysicalFamily.lean`, line 53.
- [`Article14.StablePIDFData.response_unique`](Article14/PhysicalFamily.lean#L80) — `Article14/PhysicalFamily.lean`, line 80.
- [`Article14.StablePIDFData.kernel_affine`](Article14/PhysicalFamily.lean#L155) — `Article14/PhysicalFamily.lean`, line 155.
- [`Article14.StablePIDFData.kernelL1_eq`](Article14/PhysicalFamily.lean#L188) — `Article14/PhysicalFamily.lean`, line 188.
- [`Article14.StablePIDFData.kernelDirections_injective`](Article14/PhysicalFamily.lean#L226) — `Article14/PhysicalFamily.lean`, line 226.
- [`Article14.StablePIDFData.kernel_mass_l1`](Article14/PhysicalFamily.lean#L206) — `Article14/PhysicalFamily.lean`, line 206.
- [`Article14.StablePIDFData.kernel_moments`](Article14/PhysicalFamily.lean#L134) — `Article14/PhysicalFamily.lean`, line 134.
- [`Article14.StablePIDFData.output_eq_l1KernelStep`](Article14/PhysicalFamilyOptimization.lean#L184) — `Article14/PhysicalFamilyOptimization.lean`, line 184.
- [`Article14.StablePIDFData.physical_output_monotone_iff`](Article14/PhysicalFamilyOptimization.lean#L196) — `Article14/PhysicalFamilyOptimization.lean`, line 196.
- [`Article14.StablePIDFData.physical_output_bounds`](Article14/PhysicalFamilyOptimization.lean#L204) — `Article14/PhysicalFamilyOptimization.lean`, line 204.
- [`Article14.StablePIDFData.physical_output_initial`](Article14/PhysicalFamilyOptimization.lean#L212) — `Article14/PhysicalFamilyOptimization.lean`, line 212.
- [`Article14.StablePIDFData.physical_output_final`](Article14/PhysicalFamilyOptimization.lean#L217) — `Article14/PhysicalFamilyOptimization.lean`, line 217.
- [`Article14.StablePIDFData.physical_hinf_eq_one`](Article14/PhysicalFamilyOptimization.lean#L224) — `Article14/PhysicalFamilyOptimization.lean`, line 224.
- [`Article14.StablePIDFData.mem_feasible_iff`](Article14/PhysicalFamilyOptimization.lean#L30) — `Article14/PhysicalFamilyOptimization.lean`, line 30.
- [`Article14.StablePIDFData.feasible_compact`](Article14/PhysicalFamilyOptimization.lean#L56) — `Article14/PhysicalFamilyOptimization.lean`, line 56.
- [`Article14.StablePIDFData.feasible_convex`](Article14/PhysicalFamilyOptimization.lean#L53) — `Article14/PhysicalFamilyOptimization.lean`, line 53.
- [`Article14.StablePIDFData.iae_eq_mean`](Article14/PhysicalFamilyMoments.lean#L30) — `Article14/PhysicalFamilyMoments.lean`, line 30.
- [`Article14.StablePIDFData.feasible_minimum_iae_iff_maximum_b`](Article14/PhysicalFamilyMoments.lean#L183) — `Article14/PhysicalFamilyMoments.lean`, line 183.
- [`Article14.StablePIDFData.feasible_unique_lexicographic_performance_optimizer`](Article14/PhysicalFamilyMoments.lean#L199) — `Article14/PhysicalFamilyMoments.lean`, line 199.
- [`Article14.StablePIDFData.positive_kernel_mean_gt_delay`](Article14/PhysicalFamilyMoments.lean#L103) — `Article14/PhysicalFamilyMoments.lean`, line 103.
- [`Article14.physicalRobust_fixed_controller_result`](Article14/PhysicalFamilyRobust.lean#L135) — `Article14/PhysicalFamilyRobust.lean`, line 135.
- [`Article14.physicalRobustWorstIAE_eq`](Article14/PhysicalFamilyRobust.lean#L83) — `Article14/PhysicalFamilyRobust.lean`, line 83.

<a id="result-15"></a>

## Proposition 15: Second moment and a secondary choice of c

**Paper label:** `prop:delay-second-moment`  
**Paper source:** `sections/08a_monotone_delay_miace.tex`  
**Mapping status:** Complete, with the hypotheses below.

For the actual physical family, ITAE equals half the second impulse moment and satisfies the explicit second-moment formula. The variance of the positive ordinary kernel is strictly positive, strengthening the necessary lower bound on c to a strict inequality. On the compact physical feasible set F, the smallest c at maximal b gives the unique pair with lexicographically minimal IAE and ITAE. The rational realization bridge derives DC nonsingularity, equilibrium, and the exact value of Gamma for any given minimal realization.

### Hypotheses

- Theorem 14 hypotheses and membership in F
- Gamma=L-P0'(0)/k
- Nonempty F when discussing b_star and secondary minimizer
- Positive Kd and Ki

### Mathematical obligations

- Derive second-order Taylor expansion of the actual reference transfer
- Prove ITAE equals half the second impulse moment via Tonelli
- Prove explicit q2=((Kp+1/k)*m-(1-c)*Kd-Gamma/k)/Ki
- Show strict increase in c at fixed b
- Use compact nonempty vertical slice to obtain its unique minimum-c pair
- Prove nonnegative variance of the positive unit-mass kernel and derive the stated lower bound on c

**Dependencies:** [thm:delay-fastest](#result-14).

**Reuse from Paper 13:** `MIACE.CompactParameterIntegral`, `MIACE.Transfer`.

**Proof route:** Exact HasDerivAt calculations and differentiation under the actual Laplace integral replace formal power series. Impulse moments follow by integration by parts. The nonatomic time measure strengthens the variance and the lower bound on c to strict inequalities. The compact physical vertical slice and strict monotonicity in c give the unique lexicographic optimum. Minimality is proved by divisibility of the characteristic polynomial by the reduced denominator. Equality of the minimal dimensions removes all additional factors and derives the DC equilibrium of the same realization.

**Completion criterion:** The minimal-realization constructor derives every static StablePIDFData obligation from the reduced strictly proper rational plant and any of its actual minimal finite realizations; only the article-assumed internal stability is an input. The listed physical-family theorems instantiate the numbered conclusion on that same realization.

### Exact Lean declarations

- [`Article14.RationalRealization.RationalPlant.companion_isMinimalRealization`](Article14/MinimalRealizationFamily.lean#L64) — `Article14/MinimalRealizationFamily.lean`, line 64.
- [`Article14.RationalRealization.minimal_dimension_eq`](Article14/MinimalRealizationFamily.lean#L57) — `Article14/MinimalRealizationFamily.lean`, line 57.
- [`Article14.RationalRealization.minimal_pencil_nonsingular_zero`](Article14/MinimalRealizationFamily.lean#L42) — `Article14/MinimalRealizationFamily.lean`, line 42.
- [`Article14.RationalRealization.minimalStableFamily`](Article14/MinimalRealizationFamily.lean#L104) — `Article14/MinimalRealizationFamily.lean`, line 104.
- [`Article14.RationalRealization.minimalStableFamily_gamma`](Article14/MinimalRealizationFamily.lean#L157) — `Article14/MinimalRealizationFamily.lean`, line 157.
- [`Article14.RationalRealization.minimal_normalized_transfer`](Article14/MinimalRealizationFamily.lean#L177) — `Article14/MinimalRealizationFamily.lean`, line 177.
- [`Article14.StablePIDFData.kernel_moments`](Article14/PhysicalFamily.lean#L134) — `Article14/PhysicalFamily.lean`, line 134.
- [`Article14.StablePIDFData.signed_time_error_eq_second_error_moment`](Article14/PhysicalFamilyMoments.lean#L40) — `Article14/PhysicalFamilyMoments.lean`, line 40.
- [`Article14.StablePIDFData.itae_eq_second_error_moment`](Article14/PhysicalFamilyMoments.lean#L58) — `Article14/PhysicalFamilyMoments.lean`, line 58.
- [`Article14.StablePIDFData.itae_eq_half_second_impulse_moment`](Article14/PhysicalFamilyMoments.lean#L67) — `Article14/PhysicalFamilyMoments.lean`, line 67.
- [`Article14.StablePIDFData.positive_kernel_strict_variance`](Article14/PhysicalFamilyMoments.lean#L74) — `Article14/PhysicalFamilyMoments.lean`, line 74.
- [`Article14.StablePIDFData.iae_sq_lt_twice_itae`](Article14/PhysicalFamilyMoments.lean#L86) — `Article14/PhysicalFamilyMoments.lean`, line 86.
- [`Article14.StablePIDFData.positive_kernel_strict_c_bound`](Article14/PhysicalFamilyMoments.lean#L93) — `Article14/PhysicalFamilyMoments.lean`, line 93.
- [`Article14.StablePIDFData.itae_le_iff_of_same_b`](Article14/PhysicalFamilyMoments.lean#L121) — `Article14/PhysicalFamilyMoments.lean`, line 121.
- [`Article14.StablePIDFData.feasible_unique_lexicographic_performance_optimizer`](Article14/PhysicalFamilyMoments.lean#L199) — `Article14/PhysicalFamilyMoments.lean`, line 199.

<a id="result-16"></a>

## Proposition 16: Exact settling-time feasibility

**Paper label:** `prop:delay-settling`  
**Paper source:** `sections/08a_monotone_delay_miace.tex`  
**Mapping status:** Complete, with the hypotheses below.

The exact horizon test, closed convex constraints, quasiconvexity, and attainment of the minimum settling time are instantiated on the physical L1 family and its compact feasible set. The cumulative kernel equals the actual output for every t >= 0. The rational realization bridge derives DC nonsingularity, equilibrium, and the exact value of Gamma for any given minimal realization.

### Hypotheses

- Admitted monotone designs in compact F
- 0<epsilon<1
- H>=0
- Joint continuity of actual response in time and weights
- Each feasible response tends to one

### Mathematical obligations

- Define the first threshold time using a nonempty threshold set
- Prove threshold time<=H iff y(H)>=1-epsilon for admitted designs
- Prove this horizon constraint is affine in b,c
- Prove fixed-H feasibility is convex and nested in H
- Prove a finite optimal time and an attaining pair using compactness and joint continuity

**Dependencies:** [thm:delay-fastest](#result-14).

**Proof route:** Continuity of integration on L1 gives joint continuity of the actual output. Attainment uses a compact product of the weight set and a bounded time interval instead of extracting a minimizing subsequence. Minimality is proved by divisibility of the characteristic polynomial by the reduced denominator. Equality of the minimal dimensions removes all additional factors and derives the DC equilibrium of the same realization.

**Completion criterion:** The minimal-realization constructor derives every static StablePIDFData obligation from the reduced strictly proper rational plant and any of its actual minimal finite realizations; only the article-assumed internal stability is an input. The listed physical-family theorems instantiate the numbered conclusion on that same realization.

### Exact Lean declarations

- [`Article14.RationalRealization.RationalPlant.companion_isMinimalRealization`](Article14/MinimalRealizationFamily.lean#L64) — `Article14/MinimalRealizationFamily.lean`, line 64.
- [`Article14.RationalRealization.minimal_dimension_eq`](Article14/MinimalRealizationFamily.lean#L57) — `Article14/MinimalRealizationFamily.lean`, line 57.
- [`Article14.RationalRealization.minimal_pencil_nonsingular_zero`](Article14/MinimalRealizationFamily.lean#L42) — `Article14/MinimalRealizationFamily.lean`, line 42.
- [`Article14.RationalRealization.minimalStableFamily`](Article14/MinimalRealizationFamily.lean#L104) — `Article14/MinimalRealizationFamily.lean`, line 104.
- [`Article14.RationalRealization.minimalStableFamily_gamma`](Article14/MinimalRealizationFamily.lean#L157) — `Article14/MinimalRealizationFamily.lean`, line 157.
- [`Article14.RationalRealization.minimal_normalized_transfer`](Article14/MinimalRealizationFamily.lean#L177) — `Article14/MinimalRealizationFamily.lean`, line 177.
- [`Article14.StablePIDFData.output_eq_l1KernelStep`](Article14/PhysicalFamilyOptimization.lean#L184) — `Article14/PhysicalFamilyOptimization.lean`, line 184.
- [`Article14.l1KernelStep_joint_continuous`](Article14/L1Settling.lean#L46) — `Article14/L1Settling.lean`, line 46.
- [`Article14.l1KernelStep_horizon_closed`](Article14/L1Settling.lean#L77) — `Article14/L1Settling.lean`, line 77.
- [`Article14.l1KernelStep_horizon_convex`](Article14/L1Settling.lean#L57) — `Article14/L1Settling.lean`, line 57.
- [`Article14.StablePIDFData.physical_settlingTime_le_iff`](Article14/PhysicalFamilyOptimization.lean#L261) — `Article14/PhysicalFamilyOptimization.lean`, line 261.
- [`Article14.StablePIDFData.physical_settlingTime_quasiconvex`](Article14/PhysicalFamilyOptimization.lean#L253) — `Article14/PhysicalFamilyOptimization.lean`, line 253.
- [`Article14.StablePIDFData.physical_minimum_settlingTime_exists`](Article14/PhysicalFamilyOptimization.lean#L242) — `Article14/PhysicalFamilyOptimization.lean`, line 242.

<a id="result-17"></a>

## Theorem 17: MIACE reduction within the fixed-feedback family

**Paper label:** `thm:delay-miace`  
**Paper source:** `sections/08a_monotone_delay_miace.tex`  
**Mapping status:** Complete, with the hypotheses below.

The physical set F+ is constructed from positivity of the actual impulse and the centered command. Affinity, closedness, convexity, and compactness are derived. The interval projection onto b, the exact MIACE optimum by two-sided clipping of b_alpha, and the condition for attaining the universal bound are proved. The local transfer, its DC derivative, and both signed areas follow from the same physical equations. The rational realization bridge derives DC nonsingularity, equilibrium, and the exact value of Gamma for any given minimal realization.

### Hypotheses

- Theorem 14 fixed-loop hypotheses
- Gamma>0
- alpha>0
- Actual integrable error e=1-y and centered command v=k*u-1
- F_plus nonempty for its projection/minimizer assertions

### Mathematical obligations

- Prove signed error area equals m(b)
- Prove plant signed-area balance with Gamma, including possible unstable P0 via a local-DC/denominator-cleared analytic bridge
- Import the exact MIACE gap and universal bound with integrability explicit
- Prove F_plus is compact and convex from actual affine command kernels
- Prove its b-projection is a compact interval [b_minus,b_plus]
- Complete the scalar quadratic and prove the two-sided clipping/projection optimizer
- Prove equality in the universal bound iff unprojected b_alpha lies in the attainable interval

**Dependencies:** [thm:delay-fastest](#result-14).

**Reuse from Paper 13:** `MIACE.Areas: MIACE.signal_excess`, `MIACE.Areas: MIACE.signal_equality_iff`, `MIACE.GeneralCost: MIACE.signal_miace_lower_bound`, `MIACE.GeneralCost: MIACE.signal_miace_equality_iff`, `MIACE.Transfer: MIACE.transfer_signed_balance`, `MIACE.Transfer: MIACE.paper_general_plant`, `MIACE.Algebra: MIACE.front_completion`, `MIACE.Algebra: MIACE.capped_front_minimizer`.

**Proof route:** The local transfer is derived from the actual centered equations near DC through the nonsingular matrix resolvent. Analytic continuation through positive open-loop poles is not required. The affine L1 maps for the impulse and command are constructed from three physical responses. Compactness of F+ and the interval MIACE projection follow; the rational realization bridge is proved separately. Minimality is proved by divisibility of the characteristic polynomial by the reduced denominator. Equality of the minimal dimensions removes all additional factors and derives the DC equilibrium of the same realization.

**Completion criterion:** The minimal-realization constructor derives every static StablePIDFData obligation from the reduced strictly proper rational plant and any of its actual minimal finite realizations; only the article-assumed internal stability is an input. The listed physical-family theorems instantiate the numbered conclusion on that same realization.

### Exact Lean declarations

- [`Article14.RationalRealization.RationalPlant.companion_isMinimalRealization`](Article14/MinimalRealizationFamily.lean#L64) — `Article14/MinimalRealizationFamily.lean`, line 64.
- [`Article14.RationalRealization.minimal_dimension_eq`](Article14/MinimalRealizationFamily.lean#L57) — `Article14/MinimalRealizationFamily.lean`, line 57.
- [`Article14.RationalRealization.minimal_pencil_nonsingular_zero`](Article14/MinimalRealizationFamily.lean#L42) — `Article14/MinimalRealizationFamily.lean`, line 42.
- [`Article14.RationalRealization.minimalStableFamily`](Article14/MinimalRealizationFamily.lean#L104) — `Article14/MinimalRealizationFamily.lean`, line 104.
- [`Article14.RationalRealization.minimalStableFamily_gamma`](Article14/MinimalRealizationFamily.lean#L157) — `Article14/MinimalRealizationFamily.lean`, line 157.
- [`Article14.RationalRealization.minimal_normalized_transfer`](Article14/MinimalRealizationFamily.lean#L177) — `Article14/MinimalRealizationFamily.lean`, line 177.
- [`Article14.StablePIDFData.command_affine`](Article14/PhysicalFamily.lean#L163) — `Article14/PhysicalFamily.lean`, line 163.
- [`Article14.StablePIDFData.commandL1_eq`](Article14/PhysicalFamily.lean#L193) — `Article14/PhysicalFamily.lean`, line 193.
- [`Article14.StablePIDFData.physical_transfer`](Article14/PhysicalFamily.lean#L113) — `Article14/PhysicalFamily.lean`, line 113.
- [`Article14.StablePIDFData.transfer_hasDerivAt_zero`](Article14/PhysicalFamily.lean#L244) — `Article14/PhysicalFamily.lean`, line 244.
- [`Article14.StablePIDFData.mem_positiveFeasible_iff`](Article14/PhysicalFamilyOptimization.lean#L39) — `Article14/PhysicalFamilyOptimization.lean`, line 39.
- [`Article14.StablePIDFData.positiveFeasible_compact`](Article14/PhysicalFamilyOptimization.lean#L71) — `Article14/PhysicalFamilyOptimization.lean`, line 71.
- [`Article14.StablePIDFData.positiveFeasible_convex`](Article14/PhysicalFamilyOptimization.lean#L67) — `Article14/PhysicalFamilyOptimization.lean`, line 67.
- [`Article14.StablePIDFData.positiveFeasible_projection_interval`](Article14/PhysicalFamilyOptimization.lean#L76) — `Article14/PhysicalFamilyOptimization.lean`, line 76.
- [`Article14.StablePIDFData.physical_signed_areas`](Article14/PhysicalFamilyOptimization.lean#L163) — `Article14/PhysicalFamilyOptimization.lean`, line 163.
- [`Article14.StablePIDFData.physical_miace_gap`](Article14/PhysicalFamilyOptimization.lean#L153) — `Article14/PhysicalFamilyOptimization.lean`, line 153.
- [`Article14.StablePIDFData.positiveMIACEFamily`](Article14/PhysicalFamilyOptimization.lean#L85) — `Article14/PhysicalFamilyOptimization.lean`, line 85.
- [`Article14.StablePIDFData.physical_miace_reduction`](Article14/PhysicalFamilyOptimization.lean#L107) — `Article14/PhysicalFamilyOptimization.lean`, line 107.
- [`Article14.StablePIDFData.physical_miace_every_clipped_fiber`](Article14/PhysicalFamilyOptimization.lean#L130) — `Article14/PhysicalFamilyOptimization.lean`, line 130.

<a id="result-18"></a>

## Theorem 18: A continuum of exact MIACE optima using only b,c

**Paper label:** `thm:exact-delay-family`  
**Paper source:** `sections/08b_exact_delay_example.tex`  
**Mapping status:** Complete, with the hypotheses below.

Internal stability of the actual four-dimensional PIDF realization is proved for every continuous initial history through an invertible state transformation. The impulse, output, and command are identified with the positive resolvent construction. Monotonicity, unit frequency norm, nonnegative centered command at every time, exact areas, the global MIACE minimum, and pointwise ordering follow. The endpoint q = 1 minimizes IAE over the entire square [0,1]^2, including nonmonotone competitors, as stated in the current paper.

### Hypotheses

- P_L=exp(-s/4)/(1+s)^2
- d=27/16, E0=exp(1/4), a=d-E0, rho=a/d
- Specified positive PIDF gains and shared-state realization
- 13/16<=q<=1
- 3<=alpha<=5 for the MIACE path

### Mathematical obligations

- Prove all gain signs and b(q),c(q) in [0,1]
- Prove actual Cr,Cy and prefilter identities
- Specialize/reprove positive exponentially integrable renewal resolvent
- Prove all-history internal exponential stability of the actual shared-PIDF realization, not merely a reduced transfer
- Prove q-shaped output positivity, unit gain and H-infinity norm one
- Prove complete half-line centered-command nonnegativity using the weighted compact-kernel estimate and explicit exponential bounds
- Derive exact Ae=43/16-q and Au=q-7/16 from actual signals
- Prove q_alpha remains in the allowed interval and gives global MIACE equality against every admissible settling plant response
- Prove q=1 is a global minimum-IAE design over the entire weight box, including nonmonotone and overshooting competitors
- Prove pointwise ordering along the q-path

**Dependencies:** [prop:delay-hinf](#result-13), [thm:delay-fastest](#result-14), [thm:delay-miace](#result-17).

**Reuse from Paper 13:** `MIACE.AllOrdersCompletion: MIACE.paper_all_orders_attainment at n=0,delta=1/4`, `MIACE.Theorem17Completion: MIACE.paper_rationalCascade_attainment`, `MIACE.Theorem17Signals: MIACE.rationalCascade_signals`, `MIACE.CompactRenewal`, `MIACE.WeightedRenewal`, `MIACE.CascadeCommandSigns`, `MIACE.CascadeOutput`, `MIACE.PositiveLag`.

**Proof route:** Full stability is transported through an explicit continuous linear isomorphism of the four states and all histories to the inherited rational realization. Absolute continuity and a continuous right-hand side give classical solutions; uniqueness follows from the finite-dimensional retarded estimate. Actual signals are identified with the positive construction through the physical Laplace balances and uniqueness of the transform. The time-domain prefilter identity is proved for the physical errors. The IAE optimum is extended to the entire square by integral(e) <= integral(abs(e)).

**Completion criterion:** The closed exampleData constructor uses proved all-history stability of the actual shared PIDF. Final statements apply to its actual unique physical responses; the global MIACE comparator is every integrable TransferResponse of the same delayed two-lag plant, and the IAE comparator is every weight in the whole admitted box.

### Exact Lean declarations

- [`Article14.ExactDelay.stateEquiv`](Article14/ExactDelayStateEquivalence.lean#L74) — `Article14/ExactDelayStateEquivalence.lean`, line 74.
- [`Article14.ExactDelay.stateEquiv_conjugacy`](Article14/ExactDelayStateEquivalence.lean#L112) — `Article14/ExactDelayStateEquivalence.lean`, line 112.
- [`Article14.ExactDelay.rational_full_history_stability`](Article14/ExactDelayRationalStability.lean#L12) — `Article14/ExactDelayRationalStability.lean`, line 12.
- [`Article14.ExactDelay.exact_shared_internal_stability`](Article14/ExactDelayStability.lean#L68) — `Article14/ExactDelayStability.lean`, line 68.
- [`Article14.ExactDelay.exact_shared_internal_stability_all_weights`](Article14/ExactDelayStability.lean#L73) — `Article14/ExactDelayStability.lean`, line 73.
- [`Article14.ExactDelay.exampleData`](Article14/ExactDelayStability.lean#L81) — `Article14/ExactDelayStability.lean`, line 81.
- [`Article14.ExactDelay.exact_delay_physical_family`](Article14/ExactDelayCompletion.lean#L19) — `Article14/ExactDelayCompletion.lean`, line 19.
- [`Article14.ExactDelay.exact_delay_physical_areas`](Article14/ExactDelayCompletion.lean#L40) — `Article14/ExactDelayCompletion.lean`, line 40.
- [`Article14.ExactDelay.exact_delay_reference_impulse`](Article14/ExactDelayCompletion.lean#L49) — `Article14/ExactDelayCompletion.lean`, line 49.
- [`Article14.ExactDelay.exact_delay_time_prefilter`](Article14/ExactDelayCompletion.lean#L59) — `Article14/ExactDelayCompletion.lean`, line 59.
- [`Article14.ExactDelay.exact_delay_pointwise_order`](Article14/ExactDelayCompletion.lean#L67) — `Article14/ExactDelayCompletion.lean`, line 67.
- [`Article14.ExactDelay.exact_delay_global_miace`](Article14/ExactDelayCompletion.lean#L76) — `Article14/ExactDelayCompletion.lean`, line 76.
- [`Article14.ExactDelay.exact_delay_global_iae`](Article14/ExactDelayCompletion.lean#L90) — `Article14/ExactDelayCompletion.lean`, line 90.
