# html-portfolioo
# Variational Quantum Classifier (VQC) - Mathematical Foundations
*Project:* EuroSAT Satellite Image Classification using VQC (Parameterized Quantum Circuit / PQC)  
*Algorithm Type:* Hybrid Quantum-Classical Machine Learning  
*Prepared For:* Academic Review / Research Documentation

---

## 1. Overview

The *Variational Quantum Classifier (VQC)* is a *hybrid* quantum-classical supervised learning algorithm.  
It combines:
- *Quantum encoding* of classical data (EuroSAT features from satellite images)
- *Parameterized Quantum Circuits (PQC)* to process the data
- *Classical optimization* to learn circuit parameters

The *goal* is to map an input feature vector $\mathbf{x} \in \mathbb{R}^n$ to a binary/multi-class label by finding optimal quantum circuit parameters $\boldsymbol{\theta}$.

---

## 2. Mathematical Pipeline

### 2.1 Data Encoding
Classical image features (after preprocessing) are encoded into a quantum state:

1. *Classical data*:  
   After dimensionality reduction (e.g., PCA), we get  
   $$ \mathbf{x} = (x_1, x_2, \dots, x_d) $$

2. *Feature map (encoding unitary)*:  
   The encoding is performed via a unitary transformation $U_{\phi}(\mathbf{x})$ applied to the initial state $|0\rangle^{\otimes n}$:
   $$
   |\psi_{\text{data}}\rangle = U_{\phi}(\mathbf{x}) |0\rangle^{\otimes n}
   $$

   Example: *Angle encoding* maps each $x_i$ to a rotation gate:  
   $$
   R_y(x_i) = \begin{bmatrix}
   \cos(x_i/2) & -\sin(x_i/2) \\
   \sin(x_i/2) & \cos(x_i/2)
   \end{bmatrix}
   $$

---

### 2.2 Variational Circuit
A *Parameterized Quantum Circuit (PQC)* $U(\boldsymbol{\theta})$ processes the encoded state:

$$
|\psi_{\text{model}}\rangle = U(\boldsymbol{\theta}) |\psi_{\text{data}}\rangle
$$

Here:
- $\boldsymbol{\theta} = (\theta_1, \theta_2, \dots, \theta_m)$ are *trainable parameters*
- PQC layers alternate between:
  - *Single-qubit rotations*: $R_x(\theta)$, $R_y(\theta)$, $R_z(\theta)$
  - *Entangling gates*: CNOT, CZ

---

### 2.3 Measurement & Prediction
We measure an *observable* (usually $Z$ on the first qubit):

$$
y_{\text{pred}} = \langle \psi_{\text{model}} | Z_0 | \psi_{\text{model}} \rangle
$$

This expectation value is related to *class probability*:

$$
p(\text{class}=1) = \frac{1 - y_{\text{pred}}}{2}, \quad p(\text{class}=0) = 1 - p(\text{class}=1)
$$

For *multi-class classification*, multiple qubits/measurements are used and mapped to a softmax layer.

---

## 3. Cost Function
We use a *classical loss function*. For binary classification:

$$
\mathcal{L}(\boldsymbol{\theta}) = -\frac{1}{N} \sum_{i=1}^N \left[ y_i \log p_i + (1 - y_i) \log(1 - p_i) \right]
$$

where:
- $y_i$ is the true label
- $p_i$ is the predicted probability from the quantum circuit

---

## 4. Optimization
Parameters $\boldsymbol{\theta}$ are updated *classically* using gradient-based or gradient-free optimizers.

*Parameter-Shift Rule* (for exact quantum gradients):

If $f(\theta) = \langle \psi(\theta) | \hat{O} | \psi(\theta) \rangle$,  
then:
$$
\frac{\partial f}{\partial \theta} = \frac{f(\theta + \frac{\pi}{2}) - f(\theta - \frac{\pi}{2})}{2}
$$

---

## 5. Why VQC Works for EuroSAT
- *Non-linear separability: Quantum circuits can map features into **high-dimensional Hilbert space*.
- *Entanglement*: Encodes complex correlations between spectral bands of satellite images.
- *Hybrid approach*: Works with current NISQ devices via small circuits + classical optimizers.

---
