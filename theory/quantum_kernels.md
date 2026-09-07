# Quantum Kernels

## 1. Classical kernels first

To understand quantum kernels, it is useful to start with classical machine learning.

Suppose we have data

$$
x,x'\in\mathbb{R}^d.
$$

A kernel is a function

$$
K(x,x')
$$

that measures some notion of similarity between two data points.

A classical example is the radial basis function (RBF) kernel:

$$
K(x,x')=
\exp\left(
-\frac{\|x-x'\|^2}{2\sigma^2}
\right).
$$

Two nearby points have a large similarity, while distant points have a smaller similarity.

---

# 2. The kernel trick

A kernel can be interpreted as an inner product in some feature space:

$$
K(x,x')=
\langle
\phi(x),
\phi(x')
\rangle.
$$

The important idea is that the feature transformation

$$
x\rightarrow\phi(x)
$$

does not necessarily need to be explicitly constructed.

The kernel evaluates the similarity directly.

This allows algorithms such as the support-vector machine (SVM) to work in an implicit feature space.

---

# 3. Quantum feature spaces

Quantum machine learning can replace the classical feature map by a quantum feature map:

$$
x
\rightarrow
|\phi(x)\rangle.
$$

We can then define a quantum kernel through the overlap of the quantum states.

A common definition is

$$
\boxed{
K_Q(x,x')=
\left|
\langle\phi(x)|\phi(x')\rangle
\right|^2
}
$$

where

$$
|\phi(x)\rangle=
U_\phi(x)|0\rangle^{\otimes n}.
$$

The corresponding circuit is

$$
U_\phi(x)
$$

for the first input and

$$
U_\phi(x')
$$

for the second.

---

# 4. What is the quantum computer actually doing?

This point is essential.

The quantum kernel pipeline is

$$
\boxed{
x
\rightarrow
\text{quantum feature map}
\rightarrow
\text{quantum similarity}
\rightarrow
K
\rightarrow
\text{classical ML}
}
$$

The quantum computer is used to evaluate the kernel.

The final classifier can be completely classical.

For example:

$$
\boxed{
\text{Quantum computer}
\rightarrow
K_{ij}
\rightarrow
\text{classical SVM}
}
$$

This is genuine QML because the representation/similarity computation is quantum.

---

# 5. The kernel matrix

Suppose we have $N$ training samples:

$$
x_1,x_2,\ldots,x_N.
$$

We construct the Gram matrix

$$
K\in\mathbb{R}^{N\times N}
$$

with entries

$$
K_{ij}=
K(x_i,x_j).
$$

For a quantum kernel,

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

For example,

$$
K=
\begin{pmatrix}
K(x_1,x_1)&K(x_1,x_2)&\cdots\\
K(x_2,x_1)&K(x_2,x_2)&\cdots\\
\vdots&\vdots&\ddots
\end{pmatrix}.
$$

This matrix is then provided to a classical kernel method.

---

# 6. Why does the quantum kernel have a diagonal of one?

For a normalized state,

$$
\langle\phi(x)|\phi(x)\rangle=1.
$$

Therefore,

$$
K(x,x)=
|\langle\phi(x)|\phi(x)\rangle|^2=
1.
$$

Hence the ideal fidelity-based kernel has

$$
K_{ii}=1.
$$

This is an immediate sanity check for an implementation.

---

# 7. How do we estimate the overlap?

One possibility is to use a circuit that estimates

$$
\left|
\langle\phi(x)|\phi(x')\rangle
\right|^2.
$$

Conceptually, the circuit prepares one state and compares it with another.

For small simulations, we may also compute the state vectors directly.

However, on real quantum hardware, the overlap must generally be estimated from measurements.

This introduces statistical sampling error.

Therefore, there are two distinct situations:

### Exact state-vector simulation

We may obtain

$$
K(x,x')
$$

to numerical precision.

### Shot-based execution

We estimate

$$
\hat K(x,x')
$$

from a finite number of measurements.

Thus,

$$
\hat K
\neq
K
$$

exactly because of statistical noise.

---

# 8. The quantum kernel workflow

The complete workflow is:

### Step 1 — Classical data

$$
\mathcal{D}=
\{(x_i,y_i)\}.
$$

### Step 2 — Preprocessing

Normalize the features.

### Step 3 — Quantum feature map

$$
|\phi(x_i)\rangle=
U_\phi(x_i)|0\rangle.
$$

### Step 4 — Kernel evaluation

$$
K_{ij}
=
|\langle\phi(x_i)|\phi(x_j)\rangle|^2.
$$

### Step 5 — Kernel matrix

$$
K_{\mathrm{train}}
\in
\mathbb{R}^{N\times N}.
$$

### Step 6 — Classical classifier

Train an SVM using

$$
K_{\mathrm{train}}.
$$

### Step 7 — Test data

Construct

$$
K_{\mathrm{test}}
$$

between test points and training points.

### Step 8 — Evaluation

Compare against classical baselines.

---

# 9. Why not simply use a quantum neural network?

Because quantum kernels and variational quantum models answer different questions.

A quantum kernel asks:

> Can a quantum feature space produce a useful similarity structure?

A VQC asks:

> Can a trainable quantum circuit learn a useful function?

Therefore:

$$
\boxed{
\text{Quantum kernel}
\rightarrow
\text{quantum representation}
}
$$

while

$$
\boxed{
\text{VQC}
\rightarrow
\text{quantum trainable model}
}
$$

This makes the two excellent complementary projects.

---

# 10. Quantum kernel versus classical kernel

Suppose we compare:

$$
K_Q(x,x')
$$

with

$$
K_{\mathrm{RBF}}(x,x').
$$

A meaningful experiment should evaluate both on exactly the same dataset.

For example:

| Model              | Feature representation | Classifier           |
| ------------------ | ---------------------- | -------------------- |
| Classical baseline | Raw features           | SVM + RBF            |
| Classical linear   | Raw features           | Linear SVM           |
| Quantum            | Quantum feature map    | SVM + quantum kernel |

The comparison should include more than accuracy.

Useful metrics include:

$$
\text{accuracy},
\quad
\text{precision},
\quad
\text{recall},
\quad
\text{F1},
$$

as well as

$$
\text{kernel evaluation cost},
\quad
\text{number of qubits},
\quad
\text{circuit depth},
\quad
\text{number of measurements}.
$$

---

# 11. Quantum kernel alignment

Accuracy is not the only way to evaluate a kernel.

A useful diagnostic is **kernel-target alignment**.

Let \(K\) be the kernel matrix and \(Y\) a target matrix constructed from the labels.

A normalized alignment can be written schematically as

$$
A(K,Y)=
\frac{
\langle K,Y\rangle_F
}{
\sqrt{
\langle K,K\rangle_F
\langle Y,Y\rangle_F
}
},
$$

where

$$
\langle A,B\rangle_F=
\operatorname{Tr}(A^TB)
$$

is the Frobenius inner product.

The idea is to ask:

> Does the geometry induced by the kernel reflect the class structure?

This is useful because a kernel can have interesting mathematical structure without being useful for the particular supervised task.

---

# 12. Kernel concentration

A major concern in quantum kernels is that the kernel matrix can become poorly informative.

For example, suppose many unrelated points satisfy

$$
K(x_i,x_j)\approx c
$$

for approximately the same value \(c\).

Then the kernel cannot distinguish the samples effectively.

This is an example of **kernel concentration**.

The problem illustrates an important general principle:

$$
\boxed{
\text{large Hilbert space}
\neq
\text{useful feature space}
}
$$

A high-dimensional quantum representation can still have poor task-specific geometry.

---

# 13. Trainable quantum kernels

A feature map does not necessarily have to be fixed.

We can define

$$
|\phi(x,\boldsymbol{\theta})\rangle=
U_\phi(x,\boldsymbol{\theta})|0\rangle.
$$

Then

$$
K_{\boldsymbol{\theta}}(x,x')=
\left|
\langle
\phi(x,\boldsymbol{\theta})
|
\phi(x',\boldsymbol{\theta})
\rangle
\right|^2.
$$

Now the kernel itself can be optimized.

The objective could be related to kernel-target alignment or downstream classification performance.

This creates a hybrid workflow:

$$
\boxed{
\text{quantum circuit}
+
\text{kernel}
+
\text{classical optimization}
}
$$

This is more advanced than the fixed-kernel experiment and is a natural future extension.

---

# 14. Computational cost

For $N$ training points, constructing a full kernel matrix requires approximately

$$
O(N^2)
$$

pairwise kernel evaluations.

This can become expensive even before the number of qubits becomes large.

This is an important lesson:

$$
\boxed{
\text{QML bottlenecks are not only about qubit count}
}
$$

The dataset size, circuit depth, number of measurements and optimization cost can all dominate.

---

# 15. Classical simulation versus hardware

On a classical simulator, we may evaluate the kernel accurately for relatively small circuits.

This is excellent for:

* debugging,
* studying feature maps,
* testing algorithms,
* comparing kernels,
* understanding geometry.

However, if the quantum circuit becomes classically intractable, the simulation itself becomes the bottleneck.

Real hardware could in principle evaluate the quantum feature map without explicitly representing the full $2^n$-dimensional state vector.

But real hardware introduces:

* gate errors,
* readout errors,
* decoherence,
* finite-shot noise,
* connectivity constraints.

Therefore, the transition

$$
\text{simulator}
\rightarrow
\text{real hardware}
$$

is not simply "faster".

It changes the computational and experimental regime.

---

# 16. What would count as convincing evidence of an advantage?

Suppose we obtain

$$
\mathrm{Accuracy}_{Q}=96\%
$$

and

$$
\mathrm{Accuracy}_{C}=95\%.
$$

This alone does **not** establish quantum advantage.

We would need to investigate:

* whether the classical baseline was strong;
* data preprocessing;
* hyperparameter optimization;
* training/test splits;
* statistical uncertainty;
* quantum circuit depth;
* number of shots;
* kernel evaluation cost;
* data-loading cost;
* scaling with dataset size;
* scaling with problem dimension;
* hardware noise;
* classical alternatives.

The relevant question is therefore:

$$
\boxed{
\text{Does the quantum method provide a meaningful advantage at comparable resources?}
}
$$

not simply:

$$
\boxed{
\text{Is the quantum accuracy higher?}
}
$$

---

# 17. The scientific question

The most interesting question for a quantum-kernel project is:

> **Can a quantum feature space provide a useful geometry for a learning problem that is difficult to reproduce efficiently with classical feature maps?**

This is much stronger and more scientifically meaningful than simply implementing an SVM with a quantum circuit.

---

# 18. Key takeaway

A quantum kernel combines three ideas:

$$
\boxed{
\text{classical data}
\rightarrow
\text{quantum feature map}
\rightarrow
\text{quantum similarity}
}
$$

followed by classical learning:

$$
\boxed{
K
\rightarrow
\text{SVM}
\rightarrow
\text{prediction}.
}
$$

The quantum computer does not necessarily perform the entire ML algorithm.

Instead, it provides a quantum representation or similarity measure that is inserted into an otherwise classical ML pipeline.

The central research question is whether this quantum representation provides a useful advantage in expressivity, sample efficiency, computational complexity, or robustness that cannot be obtained as efficiently with classical alternatives.

---

# `theory/quantum_data_encoding.md` — conceptual summary

The complete QML pipeline can be viewed as

$$
\boxed{
x
\rightarrow
\underbrace{U_{\mathrm{enc}}(x)}_{\text{data encoding}}
\rightarrow
\underbrace{|\phi(x)\rangle}_{\text{quantum representation}}
\rightarrow
\underbrace{\text{quantum model}}_{\text{kernel/VQC/QCNN/etc.}}
\rightarrow
\text{measurement}
\rightarrow
\hat y
}
$$

The encoding determines how classical information enters the quantum computation.

The feature map determines the resulting quantum representation.

The subsequent quantum model determines how that representation is exploited.

Keeping these three ideas separate will make the rest of the QML repository much easier to understand.
