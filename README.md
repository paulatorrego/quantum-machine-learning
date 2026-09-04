# Quantum Machine Learning

> **Theory, algorithms, implementations, and research-oriented applications of Quantum Machine Learning.**

This repository is a structured study and implementation of **Quantum Machine Learning (QML)**, combining the foundations of machine learning and quantum computing with hands-on experiments using quantum circuits, hybrid quantum-classical models, and modern QML frameworks.

The goal is not only to implement quantum machine learning algorithms, but to understand **why they work, when they may be useful, and what limits their practical advantage**.

---

## Overview

Quantum Machine Learning explores how quantum computation can be integrated with machine learning.

There are several fundamentally different ways to combine the two:

* using quantum circuits to construct feature representations;
* using quantum computers to evaluate kernel functions;
* replacing parts of neural networks with parameterized quantum circuits;
* building hybrid quantum-classical models;
* using quantum circuits as generative models;
* exploiting quantum structure in scientific and physical datasets.

A central theme is the comparison between quantum and classical approaches under comparable conditions. The objective is not to assume quantum advantage, but to investigate where quantum models provide useful capabilities and where classical methods remain superior.

---

# Learning roadmap

The repository follows the main concepts required to understand modern QML:

### 1. Machine Learning Foundations

* supervised and unsupervised learning
* regression and classification
* loss functions
* optimization
* gradient descent
* neural networks
* regularization
* generalization
* kernel methods
* support vector machines

### 2. Quantum Computing Foundations

* qubits and quantum states
* density matrices
* unitary evolution
* quantum measurements
* expectation values
* tensor products
* entanglement
* quantum channels
* parameterized quantum circuits

### 3. Quantum Data Encoding

A classical dataset must somehow be represented by a quantum state.

Different strategies are explored, including:

* basis encoding
* angle encoding
* amplitude encoding
* data re-uploading
* quantum feature maps

The choice of encoding is not merely an implementation detail. It affects circuit depth, trainability, expressivity, and the resources required to load classical data.

### 4. Quantum Feature Maps

A feature map transforms classical data

$$
x \in \mathbb{R}^d
$$

into a quantum state

$$
x \rightarrow |\phi(x)\rangle.
$$

The resulting quantum feature space can be used for classification, regression, similarity estimation, and other learning tasks.

An important question is whether this quantum feature space provides useful structure that is difficult to reproduce efficiently with classical models.

### 5. Quantum Kernel Methods

Quantum kernels use quantum circuits to estimate similarities between data points.

A typical kernel is based on

$$
K(x,x')
=
|< \phi(x)|\phi(x')>|**2.
$$

The resulting kernel matrix can then be provided to a classical learning algorithm such as a Support Vector Machine.

This creates a particularly clear hybrid workflow:

$$
\text{Classical data}
\rightarrow
\text{Quantum feature map}
\rightarrow
\text{Quantum kernel}
\rightarrow
\text{Classical ML}.
$$

The corresponding project investigates both the quantum representation and its performance relative to classical kernels.

---

# Variational Quantum Machine Learning

A second major QML paradigm replaces or augments trainable components of a classical model with a **Parameterized Quantum Circuit (PQC)**.

A typical model has the form

$$
x
\rightarrow
U_{\mathrm{enc}}(x)
\rightarrow
U(\theta)
\rightarrow
\langle O\rangle
\rightarrow
\hat{y}.
$$

The parameters

$$
\theta
$$

are optimized using a classical optimizer.

This produces a hybrid quantum-classical learning loop:

$$
\boxed{
\text{Classical data}
\rightarrow
\text{Quantum circuit}
\rightarrow
\text{Measurement}
\rightarrow
\text{Loss}
\rightarrow
\text{Classical optimizer}
\rightarrow
\theta
}
$$

The repository studies this workflow through Variational Quantum Classifiers and Quantum Neural Networks.

Particular attention is given to:

* circuit architecture;
* parameter initialization;
* gradient computation;
* optimization;
* circuit depth;
* number of qubits;
* expressivity;
* generalization;
* trainability.

---

# Quantum Neural Networks

Quantum Neural Networks can be understood as trainable quantum circuits whose parameters are optimized according to a machine learning objective.

A simple model may be written as

$$
f_\theta(x)
=
< 0|
U^\dagger(x,\theta)
O
U(x,\theta)
|0>
$$

The quantum circuit therefore acts as a differentiable component inside a larger learning pipeline.

The repository explores both fully quantum components and **hybrid quantum-classical architectures**, where classical neural networks and quantum circuits are combined.

Modern frameworks such as PennyLane are particularly suited to this workflow because they provide automatic differentiation of quantum circuits and interfaces with classical ML frameworks such as PyTorch.

---

# Quantum Convolutional Neural Networks

Quantum models can also be structured according to ideas from classical deep learning.

A **Quantum Convolutional Neural Network (QCNN)** uses local quantum operations, parameterized layers, pooling/coarse-graining ideas, and measurements to construct hierarchical representations of data.

A simplified architecture is:

$$
\text{Input}
\rightarrow
\text{Encoding}
\rightarrow
\text{Quantum convolution}
\rightarrow
\text{Pooling}
\rightarrow
\text{Quantum layers}
\rightarrow
\text{Measurement}
\rightarrow
\text{Prediction}.
$$

QCNNs provide an opportunity to investigate an important question in QML:

> Can quantum circuits exploit structured representations of data in ways that are useful for learning?

The project also considers generalization and the dependence of performance on the amount of training data.

---

# Quantum Generative Models

QML is not limited to classification and regression.

Quantum circuits can also be used to generate probability distributions or quantum states.

One example is the **Quantum Generative Adversarial Network (QGAN)**, where a generator attempts to produce samples resembling a target distribution while a discriminator learns to distinguish real and generated samples.

Conceptually:

$$
\text{Random input}
\rightarrow
G_\theta
\rightarrow
\text{Generated data}
$$

while

$$
D_\phi
:
\{\text{real},\text{generated}\}
\rightarrow
\text{prediction}.
$$

The generator and discriminator are trained adversarially.

This provides a useful introduction to:

* quantum generative models;
* adversarial optimization;
* hybrid quantum-classical training;
* probability distributions;
* sampling;
* training stability.

---

# Trainability and limitations

A central part of the repository is the study of the limitations of QML.

Implementing a quantum model is not enough to establish that it is useful.

Important questions include:

### Barren plateaus

For sufficiently deep or expressive circuits, gradients can become extremely small, making optimization difficult.

The repository investigates how gradient statistics depend on:

* number of qubits;
* circuit depth;
* ansatz;
* initialization;
* cost function.

### Noise

Real quantum hardware introduces errors and finite sampling.

The effect of noise is studied through models such as:

* depolarizing noise;
* amplitude damping;
* phase noise;
* measurement errors;
* finite-shot sampling.

### Data loading

Quantum algorithms may require significant resources to encode classical data into quantum states.

Therefore, the cost of state preparation and data encoding is considered when evaluating potential advantages.

### Classical baselines

Every meaningful QML experiment should have an appropriate classical reference.

Depending on the problem, these may include:

* logistic regression;
* SVM;
* kernel methods;
* random forests;
* neural networks;
* convolutional neural networks.

The goal is to evaluate the complete computational pipeline rather than comparing quantum and classical models only by their final accuracy.

---

# Experimental methodology

The projects follow a common experimental philosophy:

$$
\boxed{
\text{Problem}
\rightarrow
\text{Dataset}
\rightarrow
\text{Preprocessing}
\rightarrow
\text{Encoding}
\rightarrow
\text{Quantum model}
\rightarrow
\text{Training}
\rightarrow
\text{Evaluation}
\rightarrow
\text{Classical baseline}
\rightarrow
\text{Analysis}
}
$$

Whenever appropriate, experiments investigate:

* predictive performance;
* generalization;
* number of qubits;
* circuit depth;
* number of trainable parameters;
* number of measurements/shots;
* noise sensitivity;
* training cost;
* inference cost;
* scalability.

The objective is to make the experiments reproducible and scientifically interpretable.

---

# Projects

The repository currently focuses on four complementary QML paradigms:

### Project 1 — Quantum Kernel Learning

Quantum feature maps and quantum kernels for supervised learning, followed by comparison with classical kernel methods.

**Main concepts:** feature maps, quantum kernels, kernel matrices, SVMs, similarity estimation, hybrid quantum-classical learning.

### Project 2 — VQC vs Classical Neural Network

A controlled comparison between a Variational Quantum Classifier and a classical neural network.

**Main concepts:** parameterized quantum circuits, variational optimization, gradients, trainability, model capacity, generalization, and fair benchmarking.

### Project 3 — Quantum Convolutional Neural Network

A structured quantum learning model inspired by convolutional neural networks.

**Main concepts:** local quantum operations, hierarchical representations, quantum convolution, pooling, image data, and generalization.

### Project 4 — Quantum Generative Adversarial Network

A quantum generative model trained to reproduce a target probability distribution.

**Main concepts:** quantum generators, discriminators, adversarial training, sampling, probability distributions, and training stability.

---

# Beyond the core projects

Future work will extend the repository toward more research-oriented applications, including:

* quantum anomaly detection;
* Quantum Machine Learning for physical systems;
* learning and characterizing quantum thermal states;
* Quantum Machine Learning combined with tensor networks;
* noisy and hardware-oriented QML;
* scalability and resource estimation;
* quantum-classical model benchmarking.

These extensions will focus increasingly on scientific datasets and problems where the structure of the underlying quantum or physical system can be exploited.

---

# Research perspective

The central question throughout this repository is not simply:

> **Can a quantum computer perform machine learning?**

but rather:

> **When does the quantum formulation provide a useful computational or representational advantage, and what resources are required to obtain it?**

This requires considering the entire pipeline:

$$
\text{Data preparation}
+
\text{Quantum computation}
+
\text{Measurement}
+
\text{Classical optimization}
+
\text{Post-processing}.
$$

A quantum model that achieves high accuracy but requires substantially greater computational resources than a classical baseline is not automatically advantageous.

Consequently, the experiments emphasize **controlled comparisons, reproducibility, scalability, trainability, and realistic resource considerations**.

---

# Technologies

The main tools used throughout the repository include:

* **Python**
* **Qiskit**
* **PennyLane**
* **NumPy**
* **SciPy**
* **scikit-learn**
* **PyTorch**
* **Matplotlib**
* **Jupyter**

The framework choice depends on the purpose of each experiment. Qiskit provides dedicated QML building blocks such as quantum kernels and quantum neural networks, while PennyLane is particularly useful for differentiable and hybrid quantum-classical workflows.

---

# References

The theoretical development is based on textbooks, research papers, and technical documentation from the quantum computing and machine learning communities.

Selected resources include:

* Qiskit Machine Learning documentation
* PennyLane documentation and demonstrations
* Research papers on quantum kernels
* Research papers on variational quantum algorithms
* Research papers on quantum neural networks
* Research papers on quantum generative models
* Research papers on trainability and barren plateaus

Each project contains its own references and experimental methodology.

---

## Status

🚧 **Active development**

The repository is continuously evolving from foundational QML concepts toward more advanced research-oriented experiments.

The emphasis is on understanding the theory, implementing the algorithms from first principles where useful, and critically evaluating their practical potential.
