# Quantum Generative Adversarial Networks

## 1. Generative Modeling

A generative model attempts to learn a probability distribution

$$
p_{\mathrm{data}}(x)
$$

and generate samples resembling the training distribution.

A classical GAN consists of:

$$
z
\rightarrow
G_\theta(z)
\rightarrow
x_{\mathrm{fake}}
$$

and a discriminator

$$
D_\phi(x)
\rightarrow
[0,1].
$$

---

# 2. GAN Objective

The original GAN objective is

$$
\min_G\max_D
V(D,G)=
\mathbb E_{x\sim p_{\mathrm{data}}}
[\log D(x)]
+
\mathbb E_{z\sim p_z}
[\log(1-D(G(z)))].
$$

The discriminator tries to distinguish real from generated data.

The generator tries to produce samples that fool the discriminator.

---

# 3. Quantum Generator

A Quantum GAN (QGAN) replaces or augments the classical generator with a parameterized quantum circuit.

A simple quantum generator prepares

$$
|\psi(\theta)\rangle=
U(\theta)|\psi_0\rangle.
$$

Measurement in the computational basis produces samples

$$
x\sim p_\theta(x),
$$

where

$$
p_\theta(x)
=
|\langle x|\psi(\theta)\rangle|^2.
$$

The circuit therefore defines a probability distribution.

---

# 4. Quantum GAN Architecture

A conceptual architecture is

$$
\boxed{
z
\rightarrow
\text{quantum generator}
\rightarrow
\text{measurement}
\rightarrow
x_{\mathrm{fake}}
}
$$

followed by

$$
x_{\mathrm{real}},x_{\mathrm{fake}}
\rightarrow
\text{discriminator}.
$$

The discriminator can be classical or quantum.

---

# 5. Quantum Generator as a Distribution Model

Suppose the quantum state is

$$
|\psi(\theta)\rangle=
\sum_x
\alpha_x(\theta)|x\rangle.
$$

Then

$$
p_\theta(x)=
|\alpha_x(\theta)|^2.
$$

Training aims to make

$$
p_\theta(x)
\approx
p_{\mathrm{data}}(x).
$$

This is one of the most direct ways to understand a quantum generative model.

---

# 6. Entanglement and Correlations

A quantum generator can produce entangled states such as

$$
|\psi\rangle=
\sum_x\alpha_x|x\rangle.
$$

Entanglement can represent correlations between qubits.

This makes quantum circuits natural candidates for structured probability distributions.

However,

$$
\text{entanglement}
\neq
\text{automatic generative advantage}.
$$

The generated distribution still has to outperform strong classical alternatives under a fair resource comparison.

---

# 7. Quantum Discriminator

A discriminator can itself be a QNN:

$$
D_\phi(x)=
\frac{1+\langle Z\rangle}{2}.
$$

Then both generator and discriminator may be quantum.

Alternatively, a classical neural network can discriminate measured quantum samples.

---

# 8. Training Challenges

QGANs can be difficult because:

- adversarial training is unstable,
- quantum gradients are noisy,
- measurement produces finite-shot sampling noise,
- barren plateaus can appear,
- generator and discriminator must remain balanced.

Mode collapse can also occur, as in classical GANs.

---

# 9. QGAN Training Loop

A simplified loop is:

```text
Sample real data
Generate quantum samples
Train discriminator
Update quantum generator
Repeat
```

Mathematically,

$$
\phi
\leftarrow
\operatorname{optimizer}_D
(\nabla_\phi L_D),
$$

then

$$
\theta
\leftarrow
\operatorname{optimizer}_G
(\nabla_\theta L_G).
$$

---

# 10. Alternative Objectives

Instead of the original GAN loss, one can use:

- Wasserstein GAN objectives,
- least-squares GAN objectives,
- maximum mean discrepancy,
- fidelity-based losses,
- KL divergence,
- classical statistical distances.

For small quantum distributions, direct distribution metrics can be particularly informative.

---

# 11. QGAN Evaluation

Do not evaluate only discriminator accuracy.

Useful metrics include:

$$
D_{\mathrm{KL}}
(p_{\mathrm{data}}\|p_\theta),
$$

total variation distance,

$$
D_{\mathrm{TV}}=
\frac12
\sum_x
|p_{\mathrm{data}}(x)-p_\theta(x)|,
$$

and fidelity-like distribution comparisons.

Also inspect:

- mode coverage,
- sample quality,
- circuit depth,
- number of parameters,
- shots,
- training stability.

---

# 12. Classical GAN Experience

A QGAN is a particularly intuitive extension of a classical GAN:

$$
\text{classical generator}
\rightarrow
\text{quantum generator}.
$$

The conceptual adversarial game remains similar, while the generative mechanism changes.

---

# 13. QGAN Project

A strong portfolio project can begin with a small discrete distribution:

$$
p_{\mathrm{target}}(x)
$$

over $2^n$ bit strings.

Train a quantum circuit so that

$$
p_\theta(x)
\rightarrow
p_{\mathrm{target}}(x).
$$

Then compare:

- shallow versus deep circuits,
- entangling versus non-entangling circuits,
- ideal versus noisy simulation,
- different optimizers.

This is more scientifically controlled than starting with full-resolution images.
