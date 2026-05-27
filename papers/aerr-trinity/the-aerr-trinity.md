# The Ærr Trinity

## A Three-Order Framework for the Cost of Inquiry

**Art Seabra**  
**Ifthis Research**  
Philadelphia, PA  

**Status:** Preprint / working paper v0.3  
**Date:** 2026-05-27  
**DOI:** [10.5281/zenodo.TBD](https://doi.org/10.5281/zenodo.TBD)  
**License:** [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/)  
**Canonical repo:** [github.com/artseabra/aerr-frame](https://github.com/artseabra/aerr-frame)  
**Historical mirror:** [github.com/artseabra/ic-cs](https://github.com/artseabra/ic-cs)  

---

## Abstract

This paper introduces the Ærr Trinity: a unified cost-of-inquiry framework formalized at three orders: the floor, the discipline, and the drift. The first order is the Interrogation Cost Law, a law schema stating that a genuine answer to a demand must pay a cost bounded below by the structural complexity of that demand. The second order is the Ærr Coefficient, κ, a discipline parameter measuring how often a system pays the floor rather than accepting a surrogate. The third order is the Ærr Rate, the rate at which surrogate outputs accumulate when the floor is not paid.

The shared object is the surrogate stratum: the space of outputs that pay less than the cost floor, pass local inspection, and still fail to satisfy the demand they are mistaken as satisfying. The Trinity is not one equation with three names attached. It is a dependency stack: the Interrogation Cost Law defines the floor; κ measures whether that floor is paid; the Ærr Rate measures how fast unpaid floors accumulate into drift. This paper formalizes the stack, corrects the floor-weighted κ estimator, introduces count-rate and floor-weighted drift identities, gives a closed-form Chain Drift recurrence with a bounded coupling number, states the falsifiability conditions required to turn the Interrogation Cost Law from schema into domain law, and grounds the Surrogate Existence Postulate in three constructive instances from current AI research: reward model overoptimization, unfaithful chain-of-thought, and non-surjective activation steering.

The result is a qualitative-first but formally scoped framework for evaluating how systems produce convincing local success while failing global satisfaction. The paper does not claim universal empirical verification. It provides the canonical methodology and marks the remaining operationalization of demand complexity, production cost, and domain-specific cost floors as forward work.

**Keywords:** cost of inquiry, AI alignment, Goodhart, chain-of-thought faithfulness, activation steering, mechanistic interpretability, epistemic validity, surrogate outputs, local satisfaction, demand complexity, Ærr Frame

---

## 1. Introduction

AI collapsed the cost of answer-supply. It did not collapse the cost of demand-construction, verification, or genuine satisfaction. That difference is the opening problem of this paper.

A system can now produce fluent outputs faster than any institution can inspect them. Dashboards remain green. Benchmarks pass. Reward models approve. Chain-of-thought explanations sound coherent. Internal steering interventions produce the intended behavioral signature. Yet the demand may remain unpaid. The output may carry the shape of an answer while failing to satisfy what was asked. The structure is not merely error. It is a cost failure under local success.

The Ærr Trinity names that structure at three orders.

First, the Interrogation Cost Law names the floor: if an answer genuinely satisfies a demand, its production cost is bounded below by a function of that demand's structural complexity. Second, the Ærr Coefficient, κ, names discipline: the rate at which a system pays that floor across decision points. Third, the Ærr Rate names drift: the speed at which surrogate outputs accumulate when the floor is declined.

The paper's central claim is narrow and deliberately scoped. The Trinity is not offered as a completed empirical law across all domains. It is a formal schema and measurement program for a recurring failure mode: systems accepting cheaper local substitutes for globally satisfying answers. The mature empirical law will exist only in domains where demand complexity, production cost, satisfaction, and the cost-floor function are operationalized in advance.

The shared object is the surrogate stratum. A surrogate is not merely a bad answer. It is an output below the floor that can pass the local check while failing the demand. The surrogate stratum is the accumulated space of such outputs inside a system or chain of systems. The Trinity asks three questions about that object:

| Order | Quantity | Question | Answer |
|---|---|---|---|
| First | Interrogation Cost Law | Does a cost floor exist? | Yes, as a schema: satisfaction requires paying a floor. |
| Second | Ærr Coefficient, κ | Is the floor paid? | κ measures floor-payment discipline. |
| Third | Ærr Rate | What happens when it is not paid? | Surrogates accumulate at a rate governed by discipline and demand throughput. |

This paper consolidates the Trinity into one publishable statement. It also corrects two vulnerabilities in the earlier draft. First, the original count-rate identity connected κ and demand throughput to drift, but ICL appeared only implicitly through the definition of κ. This version keeps that identity and adds a floor-weighted drift identity in which the ICL floor appears explicitly. Second, the original cost-weighted κ estimator could exceed one under overpayment. This version replaces it with bounded floor-weighted compliance and floor-coverage estimators.

The result is not a metaphor. It is a measurement language for systems whose cheap channels are easier to optimize than their real demands.

### 1.1 Contributions

This paper makes five contributions.

1. It defines a bounded floor-weighted compliance estimator, $\kappa^F$, replacing a cost-weighted form that could exceed one under overpayment.
2. It introduces a floor-weighted drift identity that makes the Interrogation Cost Law explicit in dynamics through $f(C(D))$.
3. It gives a closed-form Chain Drift recurrence with the bounded coupling number $x_i=(1-\kappa_i)\eta_i\rho_i\Delta t_i$.
4. It distinguishes the Interrogation Cost Law as a law schema from falsifiable domain laws, specifying the conditions under which the schema becomes empirically refutable.
5. It grounds the Surrogate Existence Postulate in three constructive instances across behavioral, explanatory, and geometric substrates.

---

## 2. Notation

| Symbol | Read as | Meaning |
|---|---|---|
| $D$ | demand D | A demand produced by an Interrogation operation |
| $A$ | answer A | A candidate answer that genuinely satisfies a demand |
| $O$ | output O | A produced output that may or may not satisfy a demand |
| $C(D)$ | the complexity of D | Structural complexity of demand D |
| $f$ | the cost-floor function | Monotonically increasing function mapping demand complexity to minimum answer cost |
| $F_i$ | floor F sub i | The floor $f(C(D_i))$ for demand $D_i$ |
| $\text{Cost}(O)$ | the cost of O | Production cost of output O |
| $A \models D$ | A satisfies D | A genuinely satisfies demand D |
| $O \not\models D$ | O does not satisfy D | O fails to satisfy demand D |
| $\sigma_{\text{local}}(O,D)$ | local satisfaction | Local inspection score of O against D |
| $S$ | system S | A system receiving demands and producing outputs |
| $\pi_i$ | policy choice i | The system's choice at the i-th Fork: pay or decline |
| $\kappa_S$ | kappa of S | The Ærr Coefficient of system S |
| $\kappa_S(t)$ | kappa of S at t | Time-varying Ærr Coefficient |
| $\kappa_S^F$ | floor-weighted kappa | Floor-weighted compliance rate |
| $\gamma_S^F$ | floor coverage gamma | Floor-coverage ratio allowing partial payment |
| $\rho(t)$ | rho at t | Demand arrival rate |
| $\text{Ærr}(S,t)$ | Aerr rate | Count-rate accumulation of surrogates |
| $\mathcal{S}_F(S,t)$ | floor-weighted surrogate mass | Accumulated unpaid floor-mass |
| $\Delta t_i$ | delta t sub i | Duration of chain step i |
| $\eta_i$ | eta sub i | Coupling coefficient from surrogate mass at step i into later demand complexity |
| $x_i$ | coupling number x sub i | Dimensionless Chain Drift coupling number |

---

## 3. Formal status of the claims

The paper separates definitions, postulates, domain laws, empirical convergence, and forward work.

| Component | Formal status |
|---|---|
| Interrogation Cost Law | Law schema. It becomes a falsifiable domain law only after $C$, Cost, satisfaction, and $f$ are operationalized for a specified domain. |
| Surrogate clause | Contrapositive of ICL under the same domain assumptions. |
| Surrogate Existence Postulate | Separate postulate: in many nontrivial domains, under-floor outputs exist that pass local inspection while failing genuine satisfaction. |
| Ærr Coefficient, κ | Discipline estimator: the rate at which a system pays the ICL floor. |
| Floor-weighted κ | Bounded weighted estimator in $[0,1]$ for heterogeneous demand distributions. |
| Ærr Rate | Count-rate identity measuring surrogate accumulation per unit time. |
| Floor-weighted Ærr Rate | Drift identity measuring accumulated unpaid floor-mass per unit time. |
| Phantom Floor Threshold | Diagnostic regime: local checks remain green while genuine satisfaction fails. |
| Chain Drift | Additive lower bound plus multiplicative coupling recurrence when surrogates condition later demands. |
| Ærr Barrier, $B(D)$ | Forward quantity: the cost of capture or the geometry of declining a salient but wrong attractor. Not developed in this paper. |

This scoping is not cosmetic. Without it, the Interrogation Cost Law can be misread as an already completed universal law. It is not. It is the law form. Its domain instances are falsifiable only after a testable floor is specified.

---

## 4. The shared object: surrogate stratum and the Fork

Every inquiry begins with an Interrogation operation that produces a demand $D$. Satisfying that demand has a cost. At the moment of output, the system reaches a structural decision point: the Fork.

| Branch | Condition | Result |
|---|---|---|
| Pay the floor | $\text{Cost}(A) \geq f(C(D))$ | $A \models D$ |
| Decline the floor | $\text{Cost}(O) < f(C(D))$ | $O \not\models D$ |

The declined branch does not disappear. It populates the surrogate stratum: the space of outputs that cost less than the floor, pass a local check, and fail the demand.

The Trinity is three measurements of that stratum. ICL says the stratum has a boundary. κ says how often the system enters it. The Ærr Rate says how fast it fills.

Two razors stand on either side of this object.

| Razor | Operation | Role |
|---|---|---|
| Interrogation Razor | Discovery | Question only what the inquiry demands to know. It generates the demand whose floor ICL bounds. |
| Conservation Razor | Certification | Cut only what reality can afford to lose. It estimates κ and drift from outside the system. |

The asymmetry matters. Generating a demand is cheap. Certifying satisfaction is expensive. The gap between cheap generation and expensive certification is the engine of the surrogate stratum.

---

## 5. First order: the floor

Let $D$ be a demand with structural complexity $C(D)$. Let $A$ be a candidate answer with production cost $\text{Cost}(A)$. Let $A \models D$ denote genuine satisfaction.

### 5.1 Interrogation Cost Law

The Interrogation Cost Law is:

$$
A \models D \implies \text{Cost}(A) \geq f(C(D))
$$

where $f$ is monotonically increasing in $C(D)$.

Plainly: a genuine answer cannot cost less than the floor imposed by the structure of the demand.

This is a lower bound, not a maximum. Paying the floor does not mean paying infinite cost. It means paying enough for satisfaction. Overpaying can become its own surrogate when the system mistakes excess rigor for relevance. The cost floor is the minimum payment required by the demand, not a command to max every inquiry.

### 5.2 Surrogate clause

The contrapositive is the surrogate clause:

$$
\text{Cost}(O) < f(C(D)) \implies O \not\models D
$$

Any output produced below the cost floor fails to satisfy the demand, regardless of how convincing it appears under local inspection.

### 5.3 Surrogate Existence Postulate

The surrogate clause does not by itself prove that convincing under-floor outputs exist. That stronger claim is a separate postulate:

$$
\exists O : \text{Cost}(O)<f(C(D))\land \sigma_{\text{local}}(O,D)\approx1\land O\not\models D
$$

**Surrogate Existence Postulate.** For many nontrivial demands, there exist outputs below the ICL floor that pass local inspection while failing genuine satisfaction.

The gap between $\sigma_{\text{local}}\approx1$ and $O\not\models D$ is the Phantom Floor Threshold: the regime in which local gauges read green while the underlying demand remains unpaid.

### 5.4 Constructive instances

The Surrogate Existence Postulate is not introduced as a convenience. It has operational instances in current AI systems.

| Instance | Under-floor output | Local check | Genuine failure | κ-positioning |
|---|---|---|---|---|
| Reward model overoptimization | Output optimized for a proxy reward model | High proxy reward | Gold reward or true preference degrades | κ is the rate at which the pipeline performs gold-reward certification rather than accepting the proxy. |
| Unfaithful chain-of-thought | Plausible reasoning trace | Coherent explanation | Explanation fails to track the causal driver of the answer | κ is the rate at which explanations are intervention-tested rather than accepted as fluent. |
| Non-surjective activation steering | Steered activation behavior | White-box steering success | State lacks a natural prompt preimage | κ is the rate at which steered states are prompt-reachability certified rather than deployed as black-box equivalents. |

In reward model overoptimization, the demand is to produce an output genuinely preferred by humans. The local check is proxy reward. The floor is the cost of reliable preference certification through human evaluation or a robust gold reward model. The surrogate appears when proxy score increases while gold reward declines.

In unfaithful chain-of-thought, the demand is a faithful explanation of why an answer was produced. The local check is explanation plausibility. The floor is the cost of causal faithfulness testing through counterfactuals, interventions, or mechanistic verification. The surrogate appears when a fluent rationale rationalizes an answer without naming the actual driver.

In non-surjective activation steering, the demand is to show that a controlled behavior or state is reachable through the model's natural prompt pathway. The local check is steering success. The floor is prompt-reachability certification. The surrogate appears when white-box intervention produces the desired local signature while the induced state lies off the prompt-reachable manifold.

These instances span behavioral, explanatory, and geometric substrates. In each case, the cheap channel succeeds while the original demand remains unpaid.

---

## 6. Second order: the discipline

ICL says that a floor exists. It does not say whether a system pays it. The Ærr Coefficient, κ, measures floor-payment discipline.

Let a system $S$ face demands $D_1,\dots,D_n$ and choose at each Fork:

$$
\pi_i\in\{\text{pay},\text{decline}\}
$$

The count-form coefficient is:

$$
\kappa_S=\lim_{n\to\infty}\frac{1}{n}\sum_{i=1}^{n}\mathbb{1}[\pi_i=\text{pay}]
$$

when the limit exists.

Then:

$$
\kappa_S\in[0,1]
$$

A high-κ system pays the floor across most Forks. A low-κ system accepts surrogates.

### 6.1 Regimes

| κ | Regime | What is happening |
|---|---|---|
| $\kappa\approx1$ | High discipline | The system pays the floor across nearly all Forks; PFT does not deepen. |
| $0.5<\kappa<1$ | Mixed | Some demands are satisfied, some are surrogated; latent debt accrues. |
| $\kappa\approx0.5$ | Drift-permissive | Roughly half of outputs are surrogates; the PFT widens steadily. |
| $0<\kappa<0.5$ | Surrogate-dominant | Most outputs do not satisfy; local checks remain green; risk compounds. |
| $\kappa\approx0$ | Pure surrogate | The system operates in the AI-slop regime: output without satisfaction. |

κ cannot be reliably self-measured by a low-κ system. The same surrogate dynamics that produce drift can also produce surrogate self-reports about drift. Reliable estimation requires the Conservation Razor: external certification of a sample of $S$'s outputs against the relevant demand. The self-measurement limit is treated as forward work under the Halting Hole.

κ is policy-sensitive. It is not purely voluntary, because systems include incentives, architecture, measurement regimes, and institutional constraints. But it is not structural in the same sense as ICL. ICL says the floor exists. κ says whether the system pays it.

Three forces push κ downward at scale.

| Force | Mechanism |
|---|---|
| Cost asymmetry | Paying $f(C(D))$ is expensive; producing a plausible $O$ is cheap. |
| Local-check capture | $\sigma_{\text{local}}$ is observable; genuine satisfaction is harder to verify. |
| Path A drift | Operating outside the work treats the cost as external; operating inside the work treats the floor as intrinsic. |

### 6.2 Floor-weighted compliance

The unweighted κ treats all demands equally. That fails when a system pays floors for cheap demands and declines expensive ones. Define:

$$
F_i=f(C(D_i))
$$

Assume:

$$
F_i\geq0
$$

and for non-degenerate samples:

$$
\sum_{i=1}^{n}F_i>0
$$

The corrected floor-weighted κ is:

$$
\kappa_{S,n}^{F}=\frac{\sum_{i=1}^{n}F_i\cdot\mathbb{1}[\text{Cost}(O_i)\geq F_i]}{\sum_{i=1}^{n}F_i}
$$

When the limit exists:

$$
\kappa_S^F=\lim_{n\to\infty}\kappa_{S,n}^{F}
$$

#### Boundedness proof

For each $i$:

$$
0\leq F_i\cdot\mathbb{1}[\text{Cost}(O_i)\geq F_i]\leq F_i
$$

Therefore:

$$
0\leq\sum_{i=1}^{n}F_i\cdot\mathbb{1}[\text{Cost}(O_i)\geq F_i]\leq\sum_{i=1}^{n}F_i
$$

Dividing by the positive denominator gives:

$$
0\leq\kappa_{S,n}^{F}\leq1
$$

and therefore:

$$
\kappa_S^F\in[0,1]
$$

when the limit exists.

| Edge case | Result |
|---|---|
| Empty pay set | Numerator is zero, so $\kappa^F=0$. |
| All decline | Numerator is zero, so $\kappa^F=0$. |
| All pay | Numerator equals denominator, so $\kappa^F=1$. |
| All pay with overpayment | Still $\kappa^F=1$; overpayment does not inflate discipline. |
| Some $F_i=0$ | Harmless if the total denominator remains positive. |
| All $F_i=0$ | Degenerate zero-floor domain; κ is undefined or excluded. |

This is the preferred weighted form.

### 6.3 Floor coverage

If partial payment matters, define a floor-coverage ratio:

$$
\gamma_{S,n}^{F}=\frac{\sum_{i=1}^{n}\min(\text{Cost}(O_i),F_i)}{\sum_{i=1}^{n}F_i}
$$

For each $i$:

$$
0\leq\min(\text{Cost}(O_i),F_i)\leq F_i
$$

Therefore:

$$
0\leq\gamma_{S,n}^{F}\leq1
$$

and when the limit exists:

$$
\gamma_S^F\in[0,1]
$$

The difference is simple.

| Quantity | Use |
|---|---|
| $\kappa_S^F$ | Binary floor-payment discipline: did the system pay enough? |
| $\gamma_S^F$ | Partial floor coverage: how much of the floor did the system pay? |

This paper reserves κ for compliance. Coverage is useful, but it is not the same quantity.

### 6.4 Worked example: RLHF pipeline

Consider an RLHF pipeline that performs gold-reward certification on 30 percent of training updates and accepts proxy reward on the remaining 70 percent. Under uniform demand complexity, $F_i=F$, the floor-weighted compliance estimator gives:

$$
\kappa^F=0.3
$$

With an arrival rate of:

$$
\rho=1000
$$

updates per day, the count-rate Ærr identity gives:

$$
(1-\kappa)\rho=(1-0.3)1000=700
$$

surrogate updates per day. The floor-weighted drift identity gives unpaid floor-mass:

$$
700F
$$

per day. If the coupling coefficient satisfies:

$$
x=(1-\kappa)\eta\rho\Delta t\leq1
$$

then the pipeline is in the moderate-coupling regime. If downstream training compounds on those proxy-accepted updates and $x$ approaches one, carried surrogate mass approaches the doubling threshold. If $x>1$, the linear Chain Drift model predicts super-doubling geometric divergence.

The example is intentionally simple. Its purpose is to show what the gauges report once κ, ρ, and the floor are instrumented.

---

## 7. Third order: the drift

Let $S$ have time-varying discipline $\kappa_S(t)$ and demand arrival rate $\rho(t)$.

The count-rate Ærr identity is:

$$
\text{Ærr}(S,t)=\frac{d}{dt}\text{SurrogateCount}(S,t)=(1-\kappa_S(t))\rho(t)
$$

This is the rate at which surrogate outputs accumulate by count. The integral form is:

$$
\text{SurrogateCount}(S,t)=\int_0^t(1-\kappa_S(\tau))\rho(\tau)d\tau
$$

This identity binds κ and demand throughput to drift. ICL enters one level down, because κ is defined by floor payment.

### 7.1 Floor-weighted drift identity

To make the dependency on ICL explicit, define floor-weighted surrogate mass:

$$
\mathcal{S}_F(S,t)
$$

as accumulated unpaid floor-mass. Let demands at time $t$ be distributed according to $P_t(D)$. Let:

$$
\chi_S(D,t)=
\begin{cases}
1 & \text{if S pays the floor for D at t}\\
0 & \text{otherwise}
\end{cases}
$$

Then:

$$
\kappa_S^F(t)=\frac{\mathbb{E}_{D\sim P_t}[\chi_S(D,t)f(C(D))]}{\mathbb{E}_{D\sim P_t}[f(C(D))]}
$$

and:

$$
\boxed{
\frac{d}{dt}\mathcal{S}_F(S,t)=\rho(t)(1-\kappa_S^F(t))\mathbb{E}_{D\sim P_t}[f(C(D))]
}
$$

This is the explicit Trinity identity. The count-rate identity gives the number of surrogates per unit time. The floor-weighted identity gives unpaid floor-mass per unit time.

| Identity | Measures | ICL presence |
|---|---|---|
| Count-rate identity | Surrogate count | Implicit through κ |
| Floor-weighted identity | Unpaid floor-mass | Explicit through $f(C(D))$ |

The Trinity is not held together because one equation contains every symbol. It is held together by dependency: floor, payment, drift.

### 7.2 Throughput

Two systems with equal κ and different ρ have equal fractional discipline and different absolute drift. This is the AI-era pressure point. High-throughput systems decay faster at the same discipline level because they face more Forks per unit time.

AI did not lower the floor of genuine answers. It raised the arrival rate of demands and collapsed the cost of plausible substitutes.

---

## 8. Chain Drift

Most consequential failures are not single-step. They unfold across chains. A surrogate at step $i$ can feed the demand at step $i+1$, increasing ambiguity, contaminating context, or shifting the next demand away from the original target.

The additive lower bound is:

$$
\text{ChainDrift}\geq\sum_{i=1}^{n}\text{Ærr}_i\Delta t_i
$$

This is the uncoupled case. When surrogate mass propagates into later demands, the recurrence becomes multiplicative.

Let $S_i$ be accumulated surrogate mass after step $i$. Let:

$$
a_i=(1-\kappa_i)\rho_i\Delta t_i
$$

be the local surrogate increment. Let $\eta_i\geq0$ be the coupling coefficient from carried surrogate mass into the next step. Define the dimensionless coupling number:

$$
\boxed{x_i=(1-\kappa_i)\eta_i\rho_i\Delta t_i}
$$

Then:

$$
g_i=1+x_i
$$

and the recurrence is:

$$
S_{i+1}=(1+x_i)S_i+a_i
$$

### 8.1 Closed form

Iterating gives:

$$
\boxed{
S_n=S_0\prod_{j=0}^{n-1}(1+x_j)+\sum_{i=0}^{n-1}a_i\prod_{j=i+1}^{n-1}(1+x_j)
}
$$

Each local surrogate increment is carried forward through later multipliers. This is the formal content of the claim that chains of low-κ steps amplify rather than average out.

### 8.2 Regimes

| Regime | Inequality | Interpretation |
|---|---|---|
| No coupling | $x_i=0$ | Additive drift only. |
| Moderate coupling | $0<x_i\leq1$ | Per-step multiplier $g_i\in(1,2]$; bounded local amplification. |
| Doubling threshold | $x_i=1$ | Exact doubling of carried surrogate mass at that step. |
| Runaway coupling | $x_i>1$ | Super-doubling geometric divergence in the linear model. |
| Nonlinear blowup | $\eta=\eta(S)$ or $\rho=\rho(S)$ | Possible finite-time collapse; outside the linear model. |

The candidate threshold $\eta_i\rho_i\Delta t_i=1$ is only the worst-case threshold when $\kappa_i\approx0$. The true threshold is:

$$
(1-\kappa_i)\eta_i\rho_i\Delta t_i=1
$$

κ belongs inside the coupling number. A system with higher discipline can tolerate higher coupling or higher throughput before reaching the same amplification boundary.

### 8.3 Moderate-coupling asymptotics

If:

$$
0\leq x_i\leq1
$$

and:

$$
\Lambda_n=\sum_{i=0}^{n-1}x_i
$$

then:

$$
\frac{x_i}{2}\leq\log(1+x_i)\leq x_i
$$

for $x_i\in[0,1]$. Therefore:

$$
\exp\left(\frac{1}{2}\Lambda_n\right)\leq\prod_{i=0}^{n-1}(1+x_i)\leq\exp(\Lambda_n)
$$

So:

| Condition | Behavior |
|---|---|
| $\sum x_i<\infty$ | Bounded multiplicative amplification. |
| $\sum x_i\to\infty$ | Unbounded amplification. |
| $x_i=x>0$ constant | Geometric growth: $(1+x)^n$. |
| $x_i\to0$ fast enough | Finite total drift multiplier. |

For constant $x$ and constant $a$:

$$
S_n=(1+x)^nS_0+a\frac{(1+x)^n-1}{x}
$$

Thus:

$$
S_n=\Theta((1+x)^n)
$$

### 8.4 Runaway

When:

$$
x_i>1
$$

then:

$$
g_i>2
$$

In the linear model this is geometric divergence, not finite-time blowup. Finite-time blowup requires nonlinear coupling, such as:

$$
S_{i+1}=S_i+\eta S_i^p,\quad p>1
$$

or coupling terms where $\eta$, $\rho$, or downstream demand complexity grow as functions of accumulated surrogate mass.

The present paper claims only the linear result. Nonlinear Chain Drift belongs to forward work.

---

## 9. Falsifiability and domain law

With $f$ constrained only to be monotone increasing, ICL is not empirically refutable. Any apparent counterexample can be absorbed by lowering or reshaping $f$ after the fact, provided monotonicity is preserved.

Therefore the Interrogation Cost Law in this paper is a schema. It becomes a falsifiable domain law only when the following are fixed before evaluation:

| Requirement | Role |
|---|---|
| Domain $\mathcal{D}$ | Specifies the class of demands being tested. |
| Complexity measure $C(D)$ | Makes demand structure observable. |
| Cost measure $\text{Cost}(O)$ | Makes production cost observable. |
| Satisfaction oracle $V(O,D)$ | Determines whether $O\models D$. |
| Floor function or family $g(C(D))$ | Pre-registers the claimed lower bound. |
| Error tolerance $\epsilon$ | Defines acceptable measurement slack. |
| Held-out evaluation | Prevents post-hoc adjustment of the floor. |

The falsifiable domain form is:

$$
V(A,D)=1\implies\text{Cost}(A)\geq g(C(D))-\epsilon
$$

A refutation is:

$$
\exists A,D:V(A,D)=1\land\text{Cost}(A)<g(C(D))-\epsilon
$$

This is the hinge between framework and experiment. Smoothness constraints on $f$, such as Lipschitz continuity or bounded slope, are not enough if their constants are chosen after seeing the data. The floor family must be specified before evaluation.

---

## 10. Empirical convergence

This paper treats external work as convergence, not verification. The cited papers do not prove the Ærr Frame. They instantiate neighboring mechanisms under different names.

| Work | Relevance |
|---|---|
| Reward model overoptimization | Proxy reward can rise while gold reward degrades: local score succeeds while true preference fails. |
| Unfaithful chain-of-thought | Explanation plausibility can diverge from causal faithfulness. |
| Non-surjective activation steering | White-box steerability can diverge from prompt-reachable internal states. |
| Attractor Models | Iterative refinement and implicit fixed-point dynamics suggest architectural mechanisms for stabilizing refinement rather than relying on surface output alone. |

The strongest convergence is geometric. Activation steering can produce locally successful behavior while moving the residual stream off the manifold of states reachable through natural prompting. That is not identical to the Trinity, but it is a clean instance of local success diverging from the demand a reader might mistakenly infer: prompt-side reachability or black-box equivalence.

The attractor-model convergence is weaker but still relevant. Fixed-point refinement and implicit differentiation show a design pattern in which systems are not merely prompted toward better outputs but architected to refine internal states toward stable equilibria. In Ærr terms, this is adjacent to κ enforcement: raising the cost of exiting a refinement basin rather than hoping discipline appears through instruction.

These are not proofs of the Trinity. They are signals that the territory is already producing the same shape.

---

## 11. Distinction from neighboring claims

The Trinity operates near established bounds without collapsing into them.

| Neighbor | Bound on | Relation to the Trinity |
|---|---|---|
| Kolmogorov complexity | Description length of an object | Candidate ancestry for $C(D)$, not the cost relation itself. |
| Solomonoff induction | Universal inductive inference over computable hypotheses | Adjacent to inference cost and prior weighting, not demand satisfaction. |
| Chaitin program-size complexity | Program length / algorithmic randomness | Adjacent complexity lineage. |
| Minimum Description Length | Model fit and description length | Concerns model selection, not answer satisfaction under a demand. |
| Landauer principle | Physical cost of irreversible computation | Structural parallel: a hard floor, but physical rather than epistemic. |
| P vs NP / proof complexity | Computational cost and verification | Relevant to certification asymmetry, not equivalent to ICL. |
| No-free-lunch theorems | Averaged optimizer performance | Domain-different: search performance over problem classes, not satisfaction cost. |
| Goodhart effects | Proxy optimization failure | Captures local-check capture, one mechanism of the surrogate clause, but does not by itself name the cost floor or the discipline coefficient. Reward model overoptimization (§5.4) is a Goodhart instance read at the floor, discipline, and drift levels. |

The distinctive content is the coupling of demand complexity to answer cost, the measurement of floor-payment discipline, and the dynamic rate of surrogate accumulation under demand throughput.

---

## 12. Position in the Ærr Frame

The Trinity is the formal core of the Ærr Frame, but not the whole frame.

| Concept | Role | Relation to the Trinity |
|---|---|---|
| Interrogation Razor | Discovery operation | Generates the demand whose floor ICL bounds. |
| Conservation Razor | Certification operation | Estimates κ and drift from outside the system. |
| Phantom Floor Threshold | Diagnostic regime | The gap where local checks pass while satisfaction fails. |
| The Fork | Decision structure | The pay-or-decline point κ measures. |
| Path A / Path B | Stance toward inquiry | Path A accepts surrogates from outside the work; Path B pays or names the unpaid bill. |
| Causal Chain | Compounding structure | The propagation surface for Chain Drift. |
| Halting Hole | Self-measurement limit | Deferred; not formalized here. |
| Ærr Barrier, $B(D)$ | Fourth quantity candidate | Deferred; likely capture cost or geometric enforcement of the floor. |

The Trinity is the formal core of a broader framework concerned with cognition under capture pressure. This paper does not claim to deliver that broader program. It delivers a narrower mechanism: a way to name and measure the floor, the discipline, and the drift that make such a program testable.

The distinction matters. Mistaking a motivating horizon for a delivered result would reproduce the paper's own failure mode: a grand-vision-shaped surrogate with the floor unpaid.

---

## 13. Limitations

This paper has five explicit limitations.

First, $C(D)$ is not yet operationalized. Candidate measures may include graph complexity, proof length, dependency depth, annotation burden, human review time, or substrate-specific measures over Filaments and Nodes in the broader Ærr architecture.

Second, Cost is substrate-dependent. Human labor, compute, latency, cognitive effort, money, proof effort, and verification burden are not interchangeable without a conversion model.

Third, $f$ is not universal as stated. A universal monotone function would not be empirically meaningful without domain-specific grounding.

Fourth, the constructive instances are LLM- and AI-system-specific. The postulate plausibly extends to institutional drift, peer review under load, dashboard rot, and measurement-driven organizations, but that range is not established here.

Fifth, Halting Hole and nonlinear Chain Drift are deferred. The present Chain Drift model is linear multiplicative propagation. Nonlinear collapse is named only as forward work.

---

## 14. Forward work

1. Operationalize $C(D)$ in at least three domains: code review, proof verification, and peer review.
2. Define Cost for human, computational, and hybrid substrates.
3. Estimate $f$ empirically from held-out domain data.
4. Build domain ICL tests with pre-registered floor functions and refutation conditions.
5. Extend Chain Drift to nonlinear coupling where $\eta$, $\rho$, or $C(D)$ depend on accumulated surrogate mass.
6. Add non-LLM constructive instances of the Surrogate Existence Postulate.
7. Formalize the Halting Hole as a self-measurement limit.
8. Develop the Ærr Barrier, $B(D)$, as a fourth quantity: the cost of capture or the geometry of declining a salient but wrong attractor.

---

## 15. Conclusion

The Ærr Trinity names one structure at three orders.

The first order is the floor. Genuine answers must pay a cost bounded below by the structural complexity of the demand.

The second order is discipline. κ measures whether a system pays the floor or accepts a surrogate.

The third order is drift. The Ærr Rate measures how fast surrogates accumulate when discipline fails under demand throughput.

The shared object is the surrogate stratum: outputs that cost less than the floor, pass local inspection, and fail genuine satisfaction. The paper's core warning is therefore simple: local success is not satisfaction. A system can look increasingly competent while accumulating underpaid demands into a hidden stratum of failure.

The Trinity does not solve that failure. It gives it a floor, a coefficient, and a rate.

---

## References

Chaitin, G. J. 1966. On the length of programs for computing finite binary sequences. *Journal of the ACM*, 13, 547–569. DOI: 10.1145/321356.321363.

Cook, S. A. 1971. The complexity of theorem-proving procedures. *Proceedings of the Third Annual ACM Symposium on Theory of Computing*, 151–158. DOI: 10.1145/800157.805047.

Fein-Ashley, J., and Rashidinejad, P. 2026. *Solve the Loop: Attractor Models for Language and Reasoning*. arXiv:2605.12466.

Gao, L., Schulman, J., and Hilton, J. 2023. Scaling laws for reward model overoptimization. *Proceedings of the 40th International Conference on Machine Learning*, PMLR 202:10835–10866. arXiv:2210.10760.

Karp, R. M. 1972. Reducibility among combinatorial problems. In R. E. Miller and J. W. Thatcher, eds., *Complexity of Computer Computations*, 85–103. Plenum Press.

Kolmogorov, A. N. 1965/1968. Three approaches to the quantitative definition of information. *Problems of Information Transmission*, 1, 1–7; English version in *International Journal of Computer Mathematics*, 2, 157–168.

Landauer, R. 1961. Irreversibility and heat generation in the computing process. *IBM Journal of Research and Development*, 5, 183–191. DOI: 10.1147/rd.53.0183.

Mishra, A., Khashabi, D., and Liu, A. 2026. *Steered LLM Activations are Non-Surjective*. arXiv:2604.09839.

Rissanen, J. 1978. Modeling by shortest data description. *Automatica*, 14, 465–471. DOI: 10.1016/0005-1098(78)90005-5.

Solomonoff, R. J. 1964. A formal theory of inductive inference. Part I. *Information and Control*, 7, 1–22. DOI: 10.1016/S0019-9958(64)90223-2.

Solomonoff, R. J. 1964. A formal theory of inductive inference. Part II. *Information and Control*, 7, 224–254. DOI: 10.1016/S0019-9958(64)90131-7.

Turpin, M., Michael, J., Perez, E., and Bowman, S. R. 2023. Language models don't always say what they think: unfaithful explanations in chain-of-thought prompting. *Advances in Neural Information Processing Systems 36*. arXiv:2305.04388.

Wolpert, D. H., and Macready, W. G. 1997. No free lunch theorems for optimization. *IEEE Transactions on Evolutionary Computation*, 1, 67–82. DOI: 10.1109/4235.585893.

---

## AI assistance disclosure

The author used AI-assisted tools during drafting, critique, mathematical checking, and editorial refinement. The author reviewed, revised, verified, and accepts responsibility for all claims, citations, equations, and final text.

---

## How to cite

BibTeX:

```bibtex
@misc{seabra2026aerr,
  author       = {Seabra, Art},
  title        = {The {{\AE}}rr Trinity: A Three-Order Framework for the Cost of Inquiry},
  year         = {2026},
  month        = may,
  version      = {v0.3},
  doi          = {10.5281/zenodo.TBD},
  url          = {https://github.com/artseabra/aerr-frame/blob/main/papers/aerr-trinity/the-aerr-trinity.md},
  note         = {Preprint}
}
```

Plain text:

> Seabra, A. (2026). *The Ærr Trinity: A Three-Order Framework for the Cost of Inquiry* (v0.3) [Preprint]. Ifthis Research. https://github.com/artseabra/aerr-frame.

---

## License

This paper is released under the [Creative Commons Attribution 4.0 International License (CC BY 4.0)](https://creativecommons.org/licenses/by/4.0/). You may share and adapt the material with attribution.

