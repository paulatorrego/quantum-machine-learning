# Quantum Neural Networks

## 1. Definition

A Quantum Neural Network (QNN) is a quantum machine-learning model whose trainable computation is implemented, at least in part, by a parameterized quantum circuit.

A generic QNN is

$$
f_\theta(x)=
M\!\left[
U(\theta)U_{\mathrm{enc}}(x)|0\rangle
\right],
$$

where:

- $x$ is the input,
- $U_{\mathrm{enc}}(x)$ is the data-encoding circuit,
- $U(\theta)$ is the trainable quantum circuit,
- $M$ is a measurement,
- $\theta$ are trainable parameters.

A QNN is therefore best understood as a **QML model**, not as a literal biological/artificial neural network implemented gate-for-gate on a quantum computer.

---

# 2. Classical Neural Networks versus QNNs

A classical neural network can be written as

$$
h^{(\ell+1)}=
\sigma
\left(
W^{(\ell)}h^{(\ell)}
+b^{(\ell)}
\right).
$$

A QNN instead applies unitary transformations:

$$
|\psi_{\ell+1}\rangle=
U_\ell(\theta_\ell)
|\psi_\ell\rangle.
$$

The final quantum state is converted into classical information by measurement:

$$
\hat y=
\langle O\rangle.
$$

The analogy is useful, but the mathematical objects are different.

---

# 3. Hybrid QNN

A common practical architecture is

$$
x
\rightarrow
\text{classical preprocessing}
\rightarrow
U_{\mathrm{enc}}(x)
\rightarrow
U(\theta)
\rightarrow
\text{measurement}
\rightarrow
\text{classical layer}.
$$

For example,

$$
\boxed{
x
\rightarrow
\mathrm{Linear}
\rightarrow
\mathrm{Quantum\ Layer}
\rightarrow
\mathrm{Linear}
\rightarrow
\hat y
}
$$

This is often more realistic than attempting to build the entire ML pipeline quantum mechanically.

---

# 4. Quantum Layer

A quantum layer normally consists of:

1. data encoding;
2. trainable gates;
3. entangling gates;
4. measurement.

For example,

$$
U(x,\theta)=
U_{\mathrm{ent}}
U_{\mathrm{train}}(\theta)
U_{\mathrm{enc}}(x).
$$

The output may be

$$
z_i=
\langle\psi(x,\theta)|Z_i|\psi(x,\theta)\rangle.
$$

Thus a quantum layer transforms a classical feature vector into another classical feature vector.

---

# 5. QNN Training

Given a dataset

$$
\mathcal D=
\{(x_i,y_i)\}_{i=1}^{N},
$$

we minimize

$$
L(\theta)=
\frac{1}{N}
\sum_i
\ell(f_\theta(x_i),y_i).
$$

The update is

$$
\theta_{t+1}=
\theta_t-
\eta\nabla_\theta L.
$$

The gradient may be computed using:

- parameter-shift,
- adjoint differentiation,
- automatic differentiation,
- finite differences,
- stochastic gradient estimators.

---

# 6. Classification

For binary classification, a simple output is

$$
p(y=1|x)=
\frac{1+\langle Z_0\rangle}{2}.
$$

Then binary cross entropy is

$$
L=
-\frac1N
\sum_i
\left[
y_i\log p_i
+
(1-y_i)\log(1-p_i)
\right].
$$

For regression,

$$
L_{\mathrm{MSE}}=
\frac1N
\sum_i
(f_\theta(x_i)-y_i)^2.
$$

---

# 7. Multi-Qubit Outputs

A QNN can produce several measured features:

$$
z(x)=
\left(
\langle Z_0\rangle,
\langle Z_1\rangle,
\ldots,
\langle Z_{n-1}\rangle
\right).
$$

These can be fed into a classical neural network.

This leads naturally to hybrid models.

---

# 8. Entanglement

Entangling gates can generate correlations that cannot be represented as independent single-qubit states.

For example,

$$
\mathrm{CNOT}_{i,j}
$$

can create entanglement between qubits.

Entanglement may increase representational power, but more entanglement is not automatically better.

It can increase:

- depth,
- two-qubit gate count,
- noise sensitivity,
- optimization difficulty.

---

# 9. Expressivity

A QNN is expressive if its circuit family can represent a sufficiently rich set of functions.

However,

$$
\text{expressivity}\neq\text{usefulness}.
$$

An extremely expressive circuit may be difficult to train.

The practical goal is an appropriate balance between:

$$
\text{expressivity},
\quad
\text{trainability},
\quad
\text{generalization}.
$$

---

# 10. Barren Plateaus

QNNs can suffer from barren plateaus, where

$$
\mathrm{Var}(\partial_\theta L)
$$

becomes exponentially small with system size.

This makes optimization extremely difficult.

Mitigation strategies include:

- shallow ansätze,
- local losses,
- structured circuits,
- good initialization,
- layerwise training,
- symmetry-preserving circuits,
- noise reduction.

---

# 11. Data Re-uploading

Instead of encoding data only once,

$$
U(\theta)U_{\mathrm{enc}}(x),
$$

one can repeatedly encode it:

$$
U_L(\theta_L)
U_{\mathrm{enc}}(x)
\cdots
U_2(\theta_2)
U_{\mathrm{enc}}(x)
U_1(\theta_1)
U_{\mathrm{enc}}(x).
$$

This can increase the effective feature complexity without requiring one qubit per classical feature.

The trade-off is increased depth.

---

# 12. Quantum Neural Networks versus Quantum Kernels

A quantum kernel method usually performs

$$
x
\rightarrow
|\phi(x)\rangle
\rightarrow
K(x,x')
\rightarrow
\text{classical learner}.
$$

A QNN instead performs

$$
x
\rightarrow
U(x,\theta)
\rightarrow
\hat y.
$$

The kernel approach often has a fixed or separately optimized feature map, whereas the QNN learns parameters directly through a task loss.

---

# 13. QNN versus QAOA

Both can use parameterized circuits.

But:

$$
\text{QNN}
\rightarrow
\text{learn from data},
$$

whereas

$$
\text{QAOA}
\rightarrow
\text{optimize a combinatorial objective}.
$$

The distinction is the learning/problem objective, not simply the presence of trainable gates.

---

# 14. Generalization

A QNN should be evaluated on:

$$
\mathcal D_{\mathrm{train}}
\quad\text{and}\quad
\mathcal D_{\mathrm{test}}.
$$

Useful metrics include:

- accuracy,
- precision,
- recall,
- F1,
- ROC-AUC,
- mean squared error,
- calibration.

A quantum model should be compared with a classical baseline under comparable preprocessing and tuning budgets.

---

# 15. QNN Benchmarking

A good benchmark reports:

$$
(n,\;L,\;p,\;N_{\mathrm{shots}},\;\text{accuracy},\;\text{runtime})
$$

where:

- $n$: qubits,
- $L$: circuit depth,
- $p$: trainable parameters.

This prevents misleading claims based only on final accuracy.

---

# 16. QNN Limitations

Current practical limitations include:

- limited qubit counts,
- noisy gates,
- finite shots,
- measurement overhead,
- gradient estimation cost,
- barren plateaus,
- data-loading cost,
- difficult scaling.

A QNN is therefore a research model whose usefulness must be demonstrated empirically rather than assumed from its quantum nature.
