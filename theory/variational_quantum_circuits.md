# Variational Quantum Circuits and Variational Quantum Algorithms

## 1. Overview

Variational quantum methods are hybrid quantum--classical algorithms in which a parameterized quantum circuit is repeatedly evaluated and a classical optimizer updates its parameters.

A generic workflow is

$$
x \;\longrightarrow\; U(x,\theta) \;\longrightarrow\; \text{measurement}
\;\longrightarrow\; L(\theta)
\;\longrightarrow\; \text{classical optimizer}
\;\longrightarrow\; \theta'.
$$

The central idea is to replace an otherwise difficult optimization over quantum states by an optimization over the parameters of a physically realizable circuit.

This framework is important in:

- quantum machine learning (QML),
- quantum chemistry,
- many-body physics,
- quantum simulation,
- combinatorial optimization,
- near-term/noisy quantum computing.

A crucial distinction is that a variational **circuit** is not the same thing as a variational **algorithm**.

---

## 2. PQC, VQC, VQA

### 2.1 Parameterized Quantum Circuit (PQC)

A parameterized quantum circuit is a quantum circuit containing adjustable parameters:

$$
U(\theta)=U_L(\theta_L)\cdots U_2(\theta_2)U_1(\theta_1),
$$

where

$$
\theta=(\theta_1,\ldots,\theta_p)
$$

is a vector of real parameters.

The corresponding state is

$$
|\psi(\theta)\rangle=
U(\theta)|\psi_{\mathrm{in}}\rangle.
$$

A PQC is simply a circuit architecture with tunable parameters. It does not necessarily imply that the parameters are being optimized.

---

### 2.2 Variational Quantum Circuit (VQC)

The terminology is not completely standardized. In practice, a VQC usually means a parameterized circuit used inside a variational optimization procedure.

For a QML classifier,

$$
x
\rightarrow
U_{\mathrm{enc}}(x)
\rightarrow
U(\theta)
\rightarrow
\text{measurement}
\rightarrow
\hat y.
$$

The parameters are learned by minimizing a loss

$$
\theta^\star=
\arg\min_\theta L(\theta).
$$

Thus, the word *variational* emphasizes optimization over circuit parameters.

---

### 2.3 Variational Quantum Algorithm (VQA)

A VQA is the complete hybrid algorithm:

1. choose a parameterized ansatz;
2. prepare the quantum state;
3. measure observables;
4. evaluate a cost/loss function;
5. compute or estimate gradients;
6. update parameters classically;
7. repeat until convergence.

Schematically,

$$
\boxed{
\text{PQC}
+
\text{measurement}
+
\text{classical optimization}=
\text{VQA}
}
$$

Examples include:

- VQE,
- QAOA,
- many variational QML models.

A QML VQC is therefore a model architecture, whereas a VQA describes the broader hybrid optimization procedure.

---

## 3. The Variational Principle

The fundamental mathematical principle behind VQE is the Rayleigh--Ritz variational principle.

Let

$$
H|E_k\rangle=E_k|E_k\rangle,
$$

with ground-state energy

$$
E_0\le E_1\le E_2\le\cdots.
$$

For any normalized trial state \(|\psi\rangle\),

$$
\langle H\rangle_\psi=
\langle\psi|H|\psi\rangle
\ge E_0.
$$

Therefore,

$$
E_0=
\min_{|\psi\rangle}
\langle\psi|H|\psi\rangle.
$$

A variational circuit restricts the search to a parameterized family

$$
|\psi(\theta)\rangle=
U(\theta)|0\rangle,
$$

so that

$$
E(\theta)=
\langle\psi(\theta)|H|\psi(\theta)\rangle
\ge E_0.
$$

The computational problem becomes

$$
\boxed{
\theta^\star=
\arg\min_\theta E(\theta)
}
$$

and the resulting estimate is

$$
E(\theta^\star)\ge E_0.
$$

The quality of the answer depends strongly on the expressivity of the ansatz.

---

# 4. VQE: Variational Quantum Eigensolver

## 4.1 Goal

VQE estimates eigenvalues, especially ground-state energies, of a Hamiltonian

$$
H|E_0\rangle=E_0|E_0\rangle.
$$

It is especially important in:

- quantum chemistry,
- molecular electronic structure,
- spin systems,
- condensed matter,
- quantum simulation.

---

## 4.2 Hamiltonian decomposition

A Hamiltonian is generally decomposed into measurable Pauli terms:

$$
H=\sum_j c_jP_j,
$$

where

$$
P_j\in
\{I,X,Y,Z\}^{\otimes n}.
$$

Then

$$
E(\theta)=
\sum_j c_j
\langle\psi(\theta)|P_j|\psi(\theta)\rangle.
$$

Each expectation value is estimated from measurements.

This is one of the main practical costs of VQE: a complicated Hamiltonian may contain many Pauli terms.

---

## 4.3 VQE loop

The hybrid loop is

$$
\theta
\rightarrow
|\psi(\theta)\rangle
\rightarrow
\{\langle P_j\rangle\}
\rightarrow
E(\theta)
\rightarrow
\text{optimizer}
\rightarrow
\theta'.
$$

A typical algorithm is:

```text
Initialize theta
        |
        v
Prepare |psi(theta)>
        |
        v
Measure Hamiltonian terms
        |
        v
Estimate E(theta)
        |
        v
Classical optimizer
        |
        v
Update theta
        |
        +------ repeat
```

---

## 4.4 Ansatz design

An ansatz is the parameterized circuit family

$$
U(\theta).
$$

Common choices include:

### Hardware-efficient ansatz

Use gates naturally supported by a target device:

$$
U(\theta)=
\prod_{\ell=1}^{L}
U_{\mathrm{ent}}
U_{\mathrm{1q}}(\theta_\ell).
$$

Advantages:

- shallow,
- hardware-compatible,
- simple to implement.

Disadvantages:

- may be difficult to optimize,
- can suffer barren plateaus,
- may have insufficient physically motivated structure.

### Problem-inspired ansatz

Construct the circuit from the structure of the Hamiltonian.

Advantages:

- incorporates physical knowledge,
- often more efficient.

Disadvantages:

- problem-specific,
- potentially deeper or harder to design.

---

# 5. QAOA

The Quantum Approximate Optimization Algorithm is a variational algorithm designed primarily for combinatorial optimization.

It is **not primarily a QML algorithm**.

---

## 5.1 Classical optimization problem

Suppose

$$
\max_{z\in\{0,1\}^n} C(z).
$$

We encode the cost function into a diagonal Hamiltonian

$$
H_C|z\rangle=C(z)|z\rangle.
$$

We also define a mixer Hamiltonian, commonly

$$
H_M=\sum_i X_i.
$$

---

## 5.2 QAOA state

Starting from

\[
|+\rangle^{\otimes n},
\]

the depth-\(p\) QAOA state is

$$
|\boldsymbol{\gamma},\boldsymbol{\beta}\rangle=
\prod_{\ell=1}^{p}
e^{-i\beta_\ell H_M}
e^{-i\gamma_\ell H_C}
|+\rangle^{\otimes n}.
$$

The parameters

$$
\boldsymbol{\gamma}=
(\gamma_1,\ldots,\gamma_p),
\qquad
\boldsymbol{\beta}=
(\beta_1,\ldots,\beta_p)
$$

are optimized classically.

The objective can be written as

$$
C(\gamma,\beta)=
\langle
\boldsymbol{\gamma},\boldsymbol{\beta}
|
H_C
|
\boldsymbol{\gamma},\boldsymbol{\beta}
\rangle.
$$

---

## 5.3 QAOA versus VQE

| Method | Main objective |
|---|---|
| VQE | Estimate ground-state energy/eigenstate |
| QAOA | Find high-quality solutions to combinatorial optimization |
| QML VQC | Learn a prediction/model from data |

All can use parameterized circuits and classical optimization, but their objectives are different.

---

# 6. Variational QML

In QML, the circuit becomes a trainable model.

A generic model is

$$
f_\theta(x)=
\langle O\rangle_{U(\theta)U_{\mathrm{enc}}(x)|0\rangle}.
$$

Training solves

$$
\theta^\star=
\arg\min_\theta
\frac{1}{N}
\sum_{i=1}^{N}
\ell(f_\theta(x_i),y_i).
$$

The complete loop is

$$
x_i
\rightarrow
\text{encoding}
\rightarrow
U(\theta)
\rightarrow
\text{measurement}
\rightarrow
\hat y_i
\rightarrow
\mathcal L
\rightarrow
\theta'.
$$

This is analogous to a classical neural network:

$$
x
\rightarrow
\text{layers}
\rightarrow
\hat y
\rightarrow
\mathcal L
\rightarrow
\text{backpropagation}.
$$

The difference is that part of the model is implemented as a quantum circuit.

---

# 7. Data Encoding and Trainable Parameters

A QML circuit normally contains two conceptually distinct parameter sets:

$$
U(x,\theta).
$$

Here:

- $x$ contains the data;
- $\theta$ contains learned model parameters.

For angle encoding,

$$
|x\rangle=
\bigotimes_i R_y(x_i)|0\rangle.
$$

Then a trainable circuit follows:

$$
|\psi(x,\theta)\rangle=
U(\theta)U_{\mathrm{enc}}(x)|0\rangle.
$$

Confusing data features with trainable weights is a common beginner mistake.

---

# 8. Gradient Estimation

For an objective

$$
L(\theta)=
\langle\psi(\theta)|O|\psi(\theta)\rangle,
$$

we require

$$
\nabla_\theta L.
$$

## 8.1 Parameter-shift rule

For many gates generated by Pauli operators,

$$
\frac{\partial L}{\partial\theta}=
\frac{1}{2}
\left[
L\left(\theta+\frac{\pi}{2}\right)-
L\left(\theta-\frac{\pi}{2}\right)
\right].
$$

The gradient is therefore obtained through additional quantum circuit evaluations.

This is exact for the standard setting for which the parameter-shift rule applies.

---

## 8.2 Finite differences

One can also approximate

$$
\frac{\partial L}{\partial\theta}
\approx
\frac{
L(\theta+h)-L(\theta-h)
}{2h}.
$$

This is simple but introduces truncation and numerical errors.

---

## 8.3 Adjoint differentiation

For suitable simulators, adjoint differentiation can compute gradients much more efficiently than evaluating many shifted circuits.

The exact method depends on the backend and circuit structure.

---

# 9. Optimizers

Common classical optimizers include:

- gradient descent,
- Adam,
- RMSProp,
- momentum methods,
- SPSA,
- COBYLA,
- Nelder--Mead,
- L-BFGS.

Gradient-based methods are attractive when reliable gradients are available.

Derivative-free optimizers can be useful when measurements are noisy.

---

# 10. Shot Noise and Stochastic Objectives

On hardware, expectation values are estimated from a finite number $S$ of shots.

For an observable $O$,

$$
\hat{\mu}=
\frac{1}{S}
\sum_{s=1}^{S}o_s.
$$

Thus,

$$
\hat{\mu}=
\mu+\epsilon_{\mathrm{stat}},
$$

where $\epsilon_{\mathrm{stat}}$ is statistical sampling noise.

Consequently,

$$
\hat L(\theta)
$$

is stochastic even if the hardware were otherwise perfect.

Optimization becomes substantially harder.

---

# 11. Barren Plateaus

A barren plateau is a region of parameter space where gradients become extremely small.

If

$$
\mathrm{Var}
\left[
\frac{\partial L}{\partial\theta}
\right]
\sim
e^{-\alpha n},
$$

then gradients can become exponentially small as the number of qubits $n$ increases.

The consequence is

$$
\nabla_\theta L\approx0,
$$

making parameter updates ineffective.

---

## 11.1 Why barren plateaus matter

A circuit can be highly expressive but almost impossible to train.

This creates a central QML trade-off:

$$
\boxed{
\text{expressivity}
\quad\leftrightarrow\quad
\text{trainability}
}
$$

Increasing circuit depth and entanglement may increase expressive power while making optimization harder.

---

## 11.2 Sources

Barren plateaus can arise from:

- highly expressive random circuits,
- excessive circuit depth,
- global cost functions,
- noise,
- poor initialization,
- inappropriate ansatz structure.

Noise-induced barren plateaus are particularly concerning because they can make gradients exponentially suppressed.

---

## 11.3 Mitigation strategies

Useful strategies include:

- shallow circuits,
- local cost functions,
- problem-inspired ansätze,
- layerwise training,
- informed initialization,
- identity-block initialization,
- avoiding unnecessary entanglement,
- reducing noise,
- error mitigation,
- data re-uploading with controlled depth.

There is no universal solution.

---

# 12. Expressivity, Trainability, and Generalization

Three different questions must be separated:

### Expressivity

Can the circuit represent the desired functions/states?

### Trainability

Can an optimizer actually find useful parameters?

### Generalization

Does the trained model perform well on unseen data?

A very expressive model is not automatically a good model.

A useful conceptual objective is

$$
\boxed{
\text{good QML model}=
\text{expressive}
+
\text{trainable}
+
\text{generalizes}
}
$$

---

# 13. Overparameterization

Let $p$ be the number of trainable circuit parameters.

Increasing $p$ can increase model capacity but may also:

- increase optimization cost,
- increase circuit depth,
- increase noise sensitivity,
- worsen trainability.

Therefore,

$$
p\gg N
$$

is not automatically beneficial.

---

# 14. Local versus Global Cost Functions

A global cost function may depend on many or all qubits:

$$
L_{\mathrm{global}}=
\langle O_1\otimes O_2\otimes\cdots\rangle.
$$

A local cost function may involve only a small subsystem:

$$
L_{\mathrm{local}}=
\langle Z_i\rangle.
$$

Local objectives can sometimes be easier to optimize and may avoid some severe barren-plateau behavior.

---

# 15. Symmetries and Problem-Informed Ansätze

If a physical problem has a conserved quantity

$$
[H,Q]=0,
$$

it can be advantageous to design circuits that preserve the corresponding symmetry.

Examples include:

- particle-number conservation,
- parity,
- spin symmetries,
- translational constraints.

The idea is to restrict optimization to physically meaningful states.

---

# 16. VQE and Quantum Simulation

VQE is particularly relevant to quantum simulation because it attempts to prepare physically meaningful eigenstates of

$$
H.
$$

For example, for an Ising Hamiltonian

$$
H=
-J\sum_i Z_iZ_{i+1}
-g\sum_i X_i
-h\sum_i Z_i,
$$

VQE can search for an approximation to the ground state.

This differs fundamentally from thermal-state preparation.

VQE targets approximately

$$
|\psi_0\rangle,
$$

whereas thermal simulation targets

$$
\rho_\beta=
\frac{e^{-\beta H}}{Z}.
$$

Thus,

$$
\boxed{
\text{VQE: ground state}
\qquad
\text{thermal algorithms: thermal ensemble}
}
$$

These methods are complementary rather than interchangeable.

---

# 17. Practical VQE Limitations

Important costs include:

- ansatz depth,
- number of Pauli terms,
- number of shots,
- optimizer iterations,
- gradient evaluations,
- hardware noise,
- measurement grouping,
- state-preparation quality.

A small energy error does not necessarily imply that the wavefunction is accurate.

One should also inspect:

$$
|\langle\psi_{\mathrm{target}}|\psi(\theta)\rangle|^2,
$$

observables, symmetries, and relevant physical quantities.

---

# 18. Measurement Cost

Suppose

$$
H=\sum_{j=1}^{M}c_jP_j.
$$

A naive implementation estimates every

$$
\langle P_j\rangle
$$

separately.

This can make the measurement cost large.

Techniques include:

- commuting-term grouping,
- classical shadows,
- adaptive measurement,
- symmetry reduction,
- importance-based allocation of shots.

---

# 19. Quantum Resource Estimation

A serious variational study should report more than accuracy.

Useful quantities include:

- number of qubits,
- number of parameters,
- circuit depth,
- two-qubit gate count,
- number of measurements,
- number of shots,
- number of optimizer iterations,
- wall-clock/runtime,
- noise model,
- simulator/backend.

This is essential for meaningful comparisons with classical methods.

---

# 20. What Counts as a Quantum Advantage?

A quantum model outperforming a classical model on a small dataset is not sufficient to establish quantum advantage.

A convincing benchmark should consider:

$$
\text{accuracy}
+
\text{data-loading cost}
+
\text{training cost}
+
\text{inference cost}
+
\text{sampling cost}
+
\text{hardware constraints}
+
\text{classical baselines}.
$$

A classical baseline should be strong and appropriately tuned.

---

# 21. Summary

The main hierarchy is:

$$
\boxed{
\text{PQC}
\rightarrow
\text{variational circuit}
\rightarrow
\text{hybrid optimization}
}
$$

Specific applications include:

$$
\boxed{
\begin{array}{lll}
\text{VQE} &:& \text{eigenvalues / ground states}\\
\text{QAOA} &:& \text{combinatorial optimization}\\
\text{VQC/QNN} &:& \text{machine learning}
\end{array}
}
$$

The key scientific challenge is not simply building larger circuits, but finding circuits that are expressive, trainable, robust to noise, resource-efficient, and useful compared with classical alternatives.
