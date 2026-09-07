# Quantum Data Encoding

## 1. Why do we need data encoding?

A classical machine-learning problem starts with classical data,

$$
x = (x_1,x_2,\ldots,x_d)\in\mathbb{R}^d.
$$

A quantum computer, however, operates on quantum states. Therefore, before a quantum circuit can process classical data, we need a mapping

$$
x
\longrightarrow
|\psi(x)\rangle.
$$

This process is called **quantum data encoding**, **quantum data embedding**, or **quantum state preparation for classical data**.

The central idea is therefore

$$
\boxed{
\text{classical feature vector}
\rightarrow
\text{quantum state}
}
$$

The choice of this mapping is not a technical detail. It can strongly affect the expressivity, trainability, resource requirements, and ultimately the performance of a QML model.

---

## 2. The classical input

Suppose we have a binary classification dataset,

$$
\mathcal{D}=
\{(x^{(i)},y^{(i)})\}_{i=1}^{N},
$$

where

$$
x^{(i)}\in\mathbb{R}^{d}
$$

is the feature vector and

$$
y^{(i)}\in\{0,1\}
$$

is the class label.

A classical neural network might directly receive

$$
x^{(i)}
$$

as its input.

A quantum model cannot simply "receive" this vector. We must construct a quantum state depending on it:

$$
|\psi(x)\rangle=
U(x)|0\rangle^{\otimes n}.
$$

Here:

* $n$ is the number of qubits,
* $U(x)$ is a data-dependent unitary,
* $|0\rangle^{\otimes n}$ is the initial computational-basis state.

This equation is the basic mathematical definition of a quantum data encoding.

---

# 3. Encoding is not the same as learning

This distinction is fundamental.

Suppose we have

$$
x
\rightarrow
U_{\mathrm{enc}}(x)
\rightarrow
|\phi(x)\rangle.
$$

The encoding maps the classical input into a quantum state.

It does **not necessarily contain trainable parameters**.

For example,

$$
U_{\mathrm{enc}}(x)=
\bigotimes_{j=1}^{n}R_Y(x_j)
$$

may be completely fixed once \(x\) is given.

Training enters later through a parametrized circuit,

$$
U(\boldsymbol{\theta}).
$$

A typical QML model therefore looks like

$$
|0\rangle
\xrightarrow{U_{\mathrm{enc}}(x)}
|\phi(x)\rangle
\xrightarrow{U(\boldsymbol{\theta})}
|\psi(x,\boldsymbol{\theta})\rangle
\xrightarrow{\text{measurement}}
\hat y.
$$

The two roles should be kept conceptually separate:

$$
\boxed{
\text{encoding} = \text{put data into the quantum system}
}
$$

$$
\boxed{
\text{variational circuit} = \text{learn a transformation of that data}
}
$$

---

# 4. Basis encoding

In basis encoding, classical binary information is directly mapped to computational-basis states.

For example,

$$
101
$$

is encoded as

$$
|101\rangle.
$$

More generally,

$$
x=(x_1,\ldots,x_n),
\qquad
x_i\in\{0,1\},
$$

is mapped to

$$
|x_1x_2\ldots x_n\rangle.
$$

## Example

For

$$
x=(1,0,1,1),
$$

we prepare

$$
|1011\rangle.
$$

Basis encoding is conceptually simple and useful when the data is already binary.

However, it does not directly solve the problem of representing arbitrary real-valued feature vectors.

---

# 5. Angle encoding

Angle encoding is one of the most intuitive and useful approaches for small QML experiments.

Each classical feature controls a quantum rotation.

For example,

$$
x_i
\longrightarrow
R_Y(x_i).
$$

Starting from

$$
|0\rangle^{\otimes n},
$$

we can apply

$$
U_{\mathrm{enc}}(x)=
\bigotimes_{i=1}^{n}
R_Y(x_i).
$$

For one feature,

$$
R_Y(x)|0\rangle=
\cos\left(\frac{x}{2}\right)|0\rangle
+
\sin\left(\frac{x}{2}\right)|1\rangle.
$$

Thus a classical scalar becomes a parameter of a quantum rotation.

---

## 5.1 Why scaling matters

Quantum rotation gates are periodic.

For example,

$$
R_Y(\theta+2\pi)= -R_Y(\theta),
$$

where the global phase does not affect measurement probabilities.

Therefore, the numerical scale of the features matters.

If a feature has values of order

$$
10^4
$$

while another is of order

$$
10^{-2},
$$

directly using these values as rotation angles is usually inappropriate.

A common preprocessing step is therefore

$$
x_i
\rightarrow
\tilde x_i
$$

with normalized values, for example

$$
\tilde x_i\in[-\pi,\pi]
$$

or

$$
\tilde x_i\in[0,\pi].
$$

This means that **classical preprocessing is part of the QML pipeline**.

---

# 6. Amplitude encoding

Amplitude encoding uses the amplitudes of a quantum state to represent the classical vector.

Suppose

$$
x=(x_1,x_2,x_3,x_4).
$$

After normalization,

$$
\|x\|_2=1,
$$

we can encode it as

$$
|\psi(x)\rangle=
x_1|00\rangle
+
x_2|01\rangle
+
x_3|10\rangle
+
x_4|11\rangle.
$$

With \(n\) qubits, a state has

$$
2^n
$$

amplitudes.

Therefore, in principle, \(n\) qubits can represent a vector with \(2^n\) components.

This is often described as an exponential compression of the vector dimension.

However, this statement must be treated carefully.

---

## 6.1 The data-loading problem

Preparing an arbitrary amplitude-encoded state efficiently can itself be expensive.

Therefore,

$$
\boxed{
\text{exponential state dimension}
\neq
\text{automatic exponential computational advantage}
}
$$

If loading the classical data requires exponential resources, the apparent advantage can disappear.

This is one of the most important caveats in QML.

---

# 7. Data re-uploading

A single encoding layer may not provide enough expressive power.

Instead, the classical data can be injected repeatedly into the circuit.

For example,

$$
U(x,\boldsymbol{\theta})=
U_{\mathrm{enc}}(x)
U(\boldsymbol{\theta})
U_{\mathrm{enc}}(x)
U(\boldsymbol{\theta}).
$$

Schematically,

$$
x
\rightarrow
\boxed{\text{encoding}}
\rightarrow
\boxed{\text{trainable}}
\rightarrow
\boxed{\text{encoding}}
\rightarrow
\boxed{\text{trainable}}
\rightarrow
\text{measurement}.
$$

This is called **data re-uploading**.

It provides a way of constructing richer nonlinear functions of the classical input without requiring one qubit per classical feature.

---

# 8. Feature dimension versus number of qubits

A common misconception is

$$
d=n.
$$

This is not generally true.

The relationship depends on the encoding.

### Angle encoding

Often approximately

$$
d\leq n
$$

for one-shot feature encoding, although data re-uploading can change this relationship.

### Amplitude encoding

Potentially

$$
d\sim 2^n
$$

for a normalized vector.

### Basis encoding

Typically

$$
d=n
$$

for an \(n\)-bit string.

Therefore:

$$
\boxed{
\text{number of features}
\neq
\text{number of qubits}
}
$$

The mapping between them is determined by the encoding strategy.

---

# 9. Encoding and information bottlenecks

Encoding should not be evaluated only by asking:

> "How many features can I put into \(n\) qubits?"

We should also ask:

* How expensive is state preparation?
* Does the encoding preserve useful information?
* Does it create a useful geometry?
* Is it robust to noise?
* Is it compatible with the hardware?
* Is the resulting circuit trainable?
* Does it help the downstream learning problem?

The best encoding is therefore not necessarily the one with the largest theoretical information capacity.

---

# 10. Practical comparison

| Encoding          | Main idea                                  | Typical strength             | Main limitation                                   |                                           |
| ----------------- | ------------------------------------------ | ---------------------------- | ------------------------------------------------- | ----------------------------------------- |
| Basis             | $x\rightarrow$                             | $x\rangle$                    | Simple binary data                               | Limited to discrete/binary representation |
| Angle             | $x_i\rightarrow R(x_i)$                    | Simple and hardware-friendly | Usually requires several qubits for many features |                                           |
| Amplitude         | $x\rightarrow\sum_i x_i                    | i\rangle$                    | High representational density                     | State preparation can be expensive        |
| Data re-uploading | Repeated $x$-dependent gates               | Increased expressivity       | Greater circuit depth                             |                                           |
| Hybrid            | Classical preprocessing + quantum encoding | Flexible                     | Adds classical preprocessing choices              |                                           |

---

# 11. The most important practical lesson

There is no universally optimal encoding.

The right choice depends on

$$
\boxed{
\text{data}
+
\text{hardware}
+
\text{number of qubits}
+
\text{noise}
+
\text{circuit depth}
+
\text{learning task}
}
$$

Therefore, a good QML experiment should not simply choose an encoding arbitrarily.

It should ask:

> **How does the choice of encoding affect the learning problem?**

This is exactly the kind of question we will investigate in the notebooks.

---

# 12. Recommended experiment

For a small dataset, compare:

1. classical preprocessing + angle encoding;
2. angle encoding with data re-uploading;
3. amplitude encoding, when feasible.

Measure:

$$
\text{accuracy},
\qquad
\text{training time},
\qquad
\text{number of qubits},
\qquad
\text{circuit depth}.
$$

The purpose is not to declare one encoding universally superior.

The purpose is to understand the trade-offs.

---

# 13. Key takeaway

Quantum data encoding is the interface between classical information and a quantum model:

$$
\boxed{
x
\rightarrow
|\phi(x)\rangle
}
$$

It is therefore one of the most important design choices in QML.

A quantum algorithm does not become useful merely because the data has been placed in a quantum state. The encoding must create a representation that the subsequent quantum model can exploit efficiently and that can ultimately be evaluated against a meaningful classical baseline.
