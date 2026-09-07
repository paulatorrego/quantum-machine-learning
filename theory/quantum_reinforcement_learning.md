# Quantum Reinforcement Learning

## 1. Reinforcement Learning

Reinforcement learning (RL) studies an agent interacting with an environment.

At time $t$:

$$
s_t
\rightarrow
a_t
\rightarrow
r_t
\rightarrow
s_{t+1}.
$$

The agent seeks to maximize the expected discounted return

$$
G_t=
\sum_{k=0}^{\infty}
\gamma^k r_{t+k},
\qquad
0\le\gamma<1.
$$

A policy is

$$
\pi(a|s).
$$

---

# 2. Where Quantum Computing Enters

Quantum reinforcement learning (QRL) uses quantum circuits to represent or process one or more components of an RL algorithm.

Possible architectures include:

1. quantum policy,
2. quantum value function,
3. quantum state representation,
4. quantum action sampling,
5. hybrid quantum-classical agent.

There is no single universally accepted QRL architecture.

---

# 3. Quantum Policy

A quantum policy can be represented as

$$
|\psi(s,\theta)\rangle=
U(\theta)U_{\mathrm{enc}}(s)|0\rangle.
$$

Measurements define action probabilities:

$$
\pi_\theta(a|s)=
P(a|\psi(s,\theta)).
$$

The circuit therefore maps a state $s$ to a probability distribution over actions.

---

# 4. Policy Gradient

The policy-gradient objective is

$$
J(\theta)=
\mathbb E_{\tau\sim\pi_\theta}
\left[
\sum_t\gamma^t r_t
\right].
$$

A standard estimator has the form

$$
\nabla_\theta J
\approx
\frac1{B}
\sum_{\tau}
\sum_t
\nabla_\theta
\log\pi_\theta(a_t|s_t)
\,G_t.
$$

If the policy is a QNN, the quantum circuit supplies

$$
\pi_\theta(a|s).
$$

---

# 5. Quantum Value Functions

Instead of a quantum policy, one can use a quantum circuit to approximate

$$
V^\pi(s)=
\mathbb E_\pi[G_t|s_t=s]
$$

or

$$
Q^\pi(s,a)=
\mathbb E_\pi[G_t|s_t=s,a_t=a].
$$

A QNN can therefore act as a function approximator.

---

# 6. Hybrid QRL

A practical architecture is

$$
s
\rightarrow
\text{classical preprocessing}
\rightarrow
U_{\mathrm{enc}}(s)
\rightarrow
U(\theta)
\rightarrow
\text{measurement}
\rightarrow
\pi(a|s).
$$

The environment and RL loop remain classical.

Only the policy/value computation is quantum.

---

# 7. Why QRL Is Interesting

Potential motivations include:

- compact representations,
- quantum feature spaces,
- difficult stochastic policies,
- integration with quantum environments,
- quantum-native control problems.

However, none of these automatically imply a practical advantage.

---

# 8. Major Challenges

QRL inherits all QML difficulties:

- data encoding,
- barren plateaus,
- finite-shot noise,
- hardware noise,
- optimization instability,
- limited circuit depth.

RL adds:

- non-stationary data,
- high-variance gradients,
- exploration/exploitation trade-offs,
- long-horizon credit assignment.

Therefore QRL is substantially harder than supervised QML.

---

# 9. Exploration

An RL agent must explore actions.

A quantum policy naturally produces a probability distribution

$$
\pi_\theta(a|s),
$$

so sampling from the quantum output can directly implement stochastic action selection.

Nevertheless, quantum sampling is not automatically better than classical random sampling.

---

# 10. Benchmarking QRL

A credible experiment should compare:

$$
\text{QRL agent}
\quad\text{vs}\quad
\text{strong classical RL baseline}.
$$

Metrics include:

- average return,
- sample efficiency,
- convergence speed,
- variance,
- inference cost,
- circuit depth,
- number of shots.

The strongest claims require scaling experiments rather than a single toy environment.
