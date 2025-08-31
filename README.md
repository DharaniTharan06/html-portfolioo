# Variational Quantum Classifier (VQC) - Mathematical Foundations
*Project:* Soil-Based Crop Recommendation using VQC (Parameterized Quantum Circuit / PQC)  
*Algorithm Type:* Hybrid Quantum-Classical Machine Learning  
*Prepared For:* Academic Review / Research Documentation  

---

## 1. Overview

The *Variational Quantum Classifier (VQC)* is a *hybrid* quantum-classical supervised learning algorithm.  
It combines:
- *Quantum encoding* of soil features (pH, nitrogen, water-holding capacity, texture, etc.)  
- *Parameterized Quantum Circuits (PQC)* to process soil states  
- *Classical optimization* to learn circuit parameters  

The *goal* is to map an input soil feature vector $\mathbf{x} \in \mathbb{R}^d$ to a crop label (e.g., rice, maize, wheat) by finding optimal quantum circuit parameters $\boldsymbol{\theta}$.  

---

## 2. Mathematical Pipeline  

### 2.1 Data Encoding
Classical soil data are encoded into a quantum state:

1. *Classical data*:  
   Soil feature vector:  
   $$
   \mathbf{x} = (x_1, x_2, \dots, x_d)
   $$
   where:  
   - $x_1$ = pH  
   - $x_2$ = Nitrogen content  
   - $x_3$ = Water-holding capacity  
   - $x_4$ = Soil texture … etc.  

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
A *Parameterized Quantum Circuit (PQC)* $U(\boldsymbol{\theta})$ processes the encoded soil state:

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

For binary crop recommendation:
$$
p(\text{crop}=1) = \frac{1 + y_{\text{pred}}}{2}, \quad p(\text{crop}=0) = 1 - p(\text{crop}=1)
$$

For *multi-class recommendation*, multiple qubits are measured and mapped to a softmax layer.

---

## 3. Cost Function
We use a *classical loss function*. For multi-class classification (soil → crop):  

$$
\mathcal{L}(\boldsymbol{\theta}) = -\frac{1}{N} \sum_{i=1}^N \sum_{k=1}^K y_{i,k} \log(p_{i,k})
$$

where:
- $y_{i,k} = 1$ if soil sample $i$ belongs to crop $k$, else $0$  
- $p_{i,k}$ is the predicted probability for crop $k$  
- $K$ = number of crops  

---

## 4. Optimization
Parameters $\boldsymbol{\theta}$ are updated *classically* using optimizers like Adam, COBYLA, SPSA.  

*Parameter-Shift Rule* (for exact quantum gradients):  

If $f(\theta) = \langle \psi(\theta) | \hat{O} | \psi(\theta) \rangle$, then:  
$$
\frac{\partial f}{\partial \theta} = \frac{f(\theta + \frac{\pi}{2}) - f(\theta - \frac{\pi}{2})}{2}
$$

---

## 5. Why VQC Works for Soil-Crop Recommendation
- *Non-linear separability*: Quantum circuits embed soil features into a **high-dimensional Hilbert space**.  
- *Entanglement*: Captures correlations (e.g., high nitrogen + sandy soil → maize).  
- *Hybrid approach*: Works on NISQ devices with shallow circuits and classical optimizers.  
- *Direct mapping*: From soil condition vectors to optimal crop recommendation.  

---
