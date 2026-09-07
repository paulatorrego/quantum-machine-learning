# Quantum Convolutional Neural Networks

## 1. Motivation

A Quantum Convolutional Neural Network (QCNN) is a structured quantum machine-learning architecture inspired by classical convolutional neural networks.

The central ideas are:

- local operations,
- parameter sharing or repeated local structures,
- pooling/coarse-graining,
- hierarchical representation learning.

A conceptual QCNN is

$$
\boxed{
\text{encoding}
\rightarrow
\text{quantum convolution}
\rightarrow
\text{pooling}
\rightarrow
\text{quantum convolution}
\rightarrow
\text{pooling}
\rightarrow
\text{measurement}
}
$$

---

# 2. Classical CNN Analogy

A classical CNN uses

$$
\text{convolution}
\rightarrow
\text{nonlinearity}
\rightarrow
\text{pooling}.
$$

A QCNN uses parameterized quantum operations and measurements/coarse-graining.

The analogy should not be taken literally: a quantum circuit is unitary before measurement, whereas classical CNN layers are generally nonlinear maps.

---

# 3. Quantum Convolution

A local two-qubit or few-qubit unitary can act as a convolutional filter:

$$
U_{\mathrm{conv}}(\theta)=
U_L(\theta_L)\cdots U_1(\theta_1).
$$

The same or related circuit structure can be applied to different local regions.

This gives a form of parameter sharing.

---

# 4. Pooling

Pooling reduces the effective number of degrees of freedom.

A schematic operation is

$$
n
\rightarrow
n/2
\rightarrow
n/4
\rightarrow\cdots.
$$

In a quantum setting, pooling may involve:

- measurement,
- conditional operations,
- discarding qubits,
- coarse-graining transformations.

The precise construction depends on the QCNN architecture.

---

# 5. Hierarchical Representation

The architecture attempts to learn increasingly coarse representations:

$$
x
\rightarrow
h^{(1)}
\rightarrow
h^{(2)}
\rightarrow
\cdots
\rightarrow
h^{(L)}
\rightarrow
\hat y.
$$

This is particularly interesting for data with spatial or local structure.

---

# 6. QCNN for Classification

A typical workflow is

$$
x
\rightarrow
U_{\mathrm{enc}}(x)
\rightarrow
U_{\mathrm{QCNN}}(\theta)
\rightarrow
\langle Z\rangle
\rightarrow
\hat y.
$$

The loss is

$$
L(\theta)=
\frac1N
\sum_i
\ell(\hat y_i,y_i).
$$

---

# 7. Why QCNNs Are Interesting

QCNNs can be useful for studying:

- local quantum correlations,
- image-like classical data,
- quantum phase classification,
- structured QML,
- hierarchical quantum representations.

They also provide a natural bridge between QML and many-body physics.

---

# 8. Translation to Many-Body Physics

For a quantum many-body state,

$$
|\psi\rangle,
$$

a QCNN-like architecture can perform repeated coarse-graining.

This creates a conceptual relationship with renormalization-group ideas:

$$
\text{microscopic degrees of freedom}
\rightarrow
\text{coarse variables}
\rightarrow
\text{effective description}.
$$

This connection is scientifically interesting, but a QCNN should not automatically be identified with a full renormalization-group procedure.

---

# 9. Parameter Sharing

Suppose the same local unitary

$$
U(\theta)
$$

is applied to several pairs:

$$
U_{1,2}(\theta)
U_{3,4}(\theta)
U_{5,6}(\theta).
$$

The parameter count can therefore remain relatively small even as the number of qubits grows.

This is analogous to convolutional filters sharing weights.

---

# 10. Trainability

QCNNs can benefit from structured, shallow circuits.

Nevertheless, they may still encounter:

- barren plateaus,
- local minima,
- noisy gradients,
- measurement overhead.

Structure does not guarantee trainability.

---

# 11. QCNN versus Generic QNN

A generic QNN might use

$$
U_L(\theta_L)\cdots U_1(\theta_1)
$$

with arbitrary connectivity.

A QCNN imposes an architecture:

$$
\text{local operations}
+
\text{hierarchy}
+
\text{coarse-graining}.
$$

Thus QCNNs trade unrestricted expressivity for inductive structure.

---

# 12. QCNN Project Design

A good educational/research project can compare:

1. a classical CNN;
2. a small QCNN;
3. a generic QNN.

Use a simple dataset such as:

- reduced MNIST,
- synthetic image patterns,
- quantum phase labels.

Report:

- accuracy,
- number of parameters,
- qubits,
- circuit depth,
- two-qubit gates,
- training time,
- robustness to noise.

The goal should be understanding architecture and resource trade-offs, not claiming quantum advantage from a small experiment.
