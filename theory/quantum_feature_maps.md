# Quantum Feature Maps

## 1. From encoding to feature representation

Quantum machine learning often starts with a classical vector

$$
x\in\mathbb{R}^d.
$$

We encode it into a quantum state,

$$
|\phi(x)\rangle=
U_\phi(x)|0\rangle^{\otimes n}.
$$

The circuit

$$
U_\phi(x)
$$

is commonly called a **quantum feature map**.

Its purpose is to transform the original classical data into a quantum feature representation.

The central idea is

$$
\boxed{
x
\rightarrow
|\phi(x)\rangle
}
$$

where the geometry of the quantum states can be different from the geometry of the original classical data.

---

# 2. Why do we need a feature map?

Suppose two datasets are not linearly separable in the original feature space.

A classical feature transformation can map

$$
x
\rightarrow
\phi(x)
$$

such that the transformed data becomes easier to classify.

This is the basic idea behind kernel methods.

Quantum machine learning can construct a quantum feature space:

$$
x
\rightarrow
|\phi(x)\rangle.
$$

The corresponding feature space is the Hilbert space associated with the quantum system.

Because the Hilbert-space dimension scales as

$$
2^n,
$$

even a relatively small number of qubits can define a high-dimensional quantum feature space.

However, the existence of a large feature space does **not** by itself imply useful quantum advantage.

---

# 3. Feature map versus encoding

These terms are closely related but conceptually useful to distinguish.

An **encoding** specifies how classical values are inserted into a circuit.

For example,

$$
x_i\rightarrow R_Y(x_i).
$$

A **feature map** refers more broadly to the transformation

$$
x\rightarrow|\phi(x)\rangle
$$

that creates a representation intended to be useful for learning.

A feature map may therefore contain:

* data-dependent rotations,
* nonlinear data-dependent phases,
* entangling gates,
* repeated encoding,
* problem-specific structure.

Schematically,

$$
\boxed{
\text{encoding}
\subseteq
\text{feature-map construction}
}
$$

in many practical QML implementations.

---

# 4. Simple feature map

Consider

$$
U_\phi(x)=
\bigotimes_{i=1}^{n}R_Y(x_i).
$$

Then

$$
|\phi(x)\rangle=
U_\phi(x)|0\rangle^{\otimes n}.
$$

This is essentially an angle-encoding feature map.

It contains no trainable parameters.

The state is completely determined by $x$.

---

# 5. Adding entanglement

The representation can become richer by introducing entangling gates.

For example,

$$
U_\phi(x)=
U_{\mathrm{ent}}
\left(
\bigotimes_i R_Y(x_i)
\right).
$$

A simple circuit could contain

$$
R_Y(x_1),
R_Y(x_2),
\ldots,
R_Y(x_n)
$$

followed by a chain of CNOT gates:

$$
\mathrm{CNOT}_{1,2},
\mathrm{CNOT}_{2,3},
\ldots
$$

The resulting state may contain correlations between different features.

This gives an important conceptual distinction:

$$
\boxed{
\text{local encoding}
\rightarrow
\text{feature representation}
}
$$

versus

$$
\boxed{
\text{local encoding + entanglement}
\rightarrow
\text{correlated quantum representation}
}
$$

---

# 6. Nonlinear quantum feature maps

A feature map does not need to depend only linearly on the features.

For example, a data-dependent phase may contain terms such as

$$
x_i x_j.
$$

A schematic feature map could therefore generate phases of the form




The second term couples features.

This is conceptually similar to introducing nonlinear interactions in classical feature engineering.

---

# 7. Quantum feature maps and Hilbert spaces

The most important mathematical object is

$$
|\phi(x)\rangle.
$$

Each classical data point corresponds to a vector in Hilbert space.

Thus,

$$
x^{(1)}
\rightarrow
|\phi(x^{(1)})\rangle,
$$

$$
x^{(2)}
\rightarrow
|\phi(x^{(2)})\rangle.
$$

The classical dataset has therefore acquired a quantum geometry.

For two points, a natural similarity measure is their overlap:

$$
\langle\phi(x)|\phi(x')\rangle.
$$

The squared magnitude is

$$
K(x,x')=
\left|
\langle\phi(x)|\phi(x')\rangle
\right|^2.
$$

This is the quantum kernel.

Therefore:

$$
\boxed{
\text{feature map}
\rightarrow
\text{quantum geometry}
\rightarrow
\text{quantum kernel}
}
$$

This connection is central to quantum kernel methods.

---

# 8. Trainable versus fixed feature maps

A feature map can be fixed:

$$
U_\phi(x).
$$

Or it can contain trainable parameters:

$$
U_\phi(x,\boldsymbol{\theta}).
$$

The first case produces a fixed quantum feature space.

The second produces a **trainable feature representation**.

For example,

$$
|\phi(x,\boldsymbol{\theta})\rangle=
U_\phi(x,\boldsymbol{\theta})|0\rangle.
$$

The parameters can be optimized using a loss function.

This distinction is useful:

$$
\boxed{
\text{fixed feature map}
\rightarrow
\text{representation chosen in advance}
}
$$

$$
\boxed{
\text{trainable feature map}
\rightarrow
\text{representation learned from data}
}
$$

---

# 9. Feature maps in quantum kernels

The quantum-kernel workflow is

$$
x
\rightarrow
U_\phi(x)
\rightarrow
|\phi(x)\rangle
\rightarrow
K(x,x').
$$

For a dataset with $N$ samples, we construct a Gram matrix

$$
K_{ij}=
K(x_i,x_j).
$$

For the fidelity-based kernel,

$$
K_{ij}=
\left|
\langle
\phi(x_i)
|
\phi(x_j)
\rangle
\right|^2.
$$

The resulting matrix is then given to a classical kernel algorithm such as an SVM.

---

# 10. Feature maps in variational QML

Feature maps are also used in variational circuits.

A typical model is

$$
|0\rangle
\xrightarrow{U_\phi(x)}
|\phi(x)\rangle
\xrightarrow{U(\boldsymbol{\theta})}
|\psi(x,\boldsymbol{\theta})\rangle
\xrightarrow{\text{measurement}}
\hat y.
$$

Here:

* $U_\phi(x)$ encodes the input;
* $U(\boldsymbol{\theta})$ is trainable;
* measurements generate classical outputs;
* a classical optimizer updates $\boldsymbol{\theta}$.

This is the standard hybrid QML picture.

---

# 11. Expressivity

A useful feature map should ideally represent the distinctions between data points that matter for the learning task.

If many different inputs produce almost indistinguishable states,

$$
|\phi(x)\rangle
\approx
|\phi(x')\rangle,
$$

the feature map may lose useful information.

On the other hand, an extremely complicated feature map may require:

* many gates,
* large depth,
* many measurements,
* difficult optimization,
* greater sensitivity to noise.

Therefore:

$$
\boxed{
\text{more expressive}
\neq
\text{automatically better}
}
$$

This trade-off is central to QML.

---

# 12. Feature-map depth

Suppose

$$
U_\phi(x)=
U_L(x)\cdots U_2(x)U_1(x).
$$

Increasing $L$ can increase the complexity of the representation.

But deeper circuits also mean:

$$
\text{more gates}
\rightarrow
\text{more opportunities for errors}.
$$

On noisy hardware,

$$
\text{deeper circuit}
\rightarrow
\text{lower effective fidelity}
$$

in many situations.

Therefore, feature-map depth should be treated as an experimental hyperparameter.

---

# 13. Feature-map design as an experiment

A good QML experiment should compare feature maps rather than assuming one is optimal.

For example:

### Map A

$$
U_A(x)=
\prod_iR_Y(x_i).
$$

### Map B

$$
U_B(x)
=
U_{\mathrm{ent}}
\prod_iR_Y(x_i).
$$

### Map C

$$
U_C(x)=
U_{\mathrm{ent}}
U_{\mathrm{enc}}(x)
U_{\mathrm{ent}}
U_{\mathrm{enc}}(x).
$$

Then compare:

* classification accuracy,
* kernel alignment,
* circuit depth,
* number of two-qubit gates,
* robustness to noise,
* training or evaluation cost.

This turns feature-map choice into a scientific question rather than a coding choice.

---

# 14. The danger of arbitrary feature-map design

It is easy to construct increasingly complicated circuits.

But a more complicated circuit does not automatically produce a better representation.

A feature map should ideally have a motivation:

* physical structure,
* locality,
* data structure,
* known correlations,
* symmetry,
* problem geometry,
* hardware constraints.

This is particularly important for a research-oriented repository.

---

# 15. Quantum advantage is not guaranteed

A quantum feature map can produce a high-dimensional representation.

That does not imply

$$
\text{quantum ML}
>
\text{classical ML}.
$$

A classical model may represent the same useful structure more efficiently.

Therefore, experiments should compare quantum feature maps against strong classical alternatives.

For example:

$$
\boxed{
\text{Quantum kernel}
\quad\text{vs}\quad
\text{RBF kernel}
}
$$

and

$$
\boxed{
\text{quantum feature map}
\quad\text{vs}\quad
\text{classical feature engineering}
}
$$

The relevant question is not simply whether the quantum model obtains high accuracy, but whether it provides a meaningful computational or statistical advantage under a fair resource comparison.

---

# 16. Key takeaway

A quantum feature map constructs a quantum representation of classical data:

$$
\boxed{
x
\mapsto
|\phi(x)\rangle
}
$$

The feature map determines the geometry in which the subsequent learning problem takes place.

Its quality depends on a balance between:

$$
\boxed{
\text{expressivity}
+
\text{trainability}
+
\text{noise robustness}
+
\text{resource efficiency}
}
$$

This is why feature-map design is one of the central research questions in quantum machine learning.

