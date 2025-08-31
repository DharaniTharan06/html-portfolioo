# html-portfolioo
# Variational Quantum Classifier (VQC) – Mathematical Foundations

**Project:** Soil-Based Crop Recommendation using VQC (Parameterized Quantum Circuit / PQC)  
**Algorithm Type:** Hybrid Quantum-Classical Machine Learning  
**Prepared For:** Academic Review / Research Documentation  

---

## 1. Overview

The **Variational Quantum Classifier (VQC)** is a hybrid quantum-classical supervised learning algorithm applied to **crop recommendation**.

It combines:
- **Quantum encoding** of soil properties (pH, nitrogen, water-holding capacity, texture, etc.)  
- **Parameterized Quantum Circuits (PQC)** to process the soil feature states  
- **Classical optimization** to learn quantum circuit parameters  

The goal is to map an input soil feature vector:

\[
x \in \mathbb{R}^d
\]

to a **crop label** (e.g., rice, maize, wheat) by finding optimal quantum circuit parameters \(\theta\).

---

## 2. Mathematical Pipeline

### 2.1 Data Encoding

Soil features are encoded into a quantum state.  

Classical soil data:

\[
x = (x_1, x_2, \dots, x_d)
\]

where:  
- \(x_1 = \text{pH}\)  
- \(x_2 = \text{Nitrogen \%}\)  
- \(x_3 = \text{Water holding capacity}\)  
- etc.  

**Feature map (encoding unitary):**

\[
|\psi_{\text{data}}\rangle = U_{\phi}(x) |0\rangle^{\otimes n}
\]

**Example (Angle Encoding):**

Each soil feature \(x_i\) is mapped to a qubit rotation:

\[
R_y(x_i) =
\begin{bmatrix}
\cos(x_i/2) & -\sin(x_i/2) \\
\sin(x_i/2) & \cos(x_i/2)
\end{bmatrix}
\]

---

### 2.2 Variational Circuit (PQC)

A **Parameterized Quantum Circuit (PQC)** processes the encoded soil state:

\[
|\psi_{\text{model}}\rangle = U(\theta) |\psi_{\text{data}}\rangle
\]

Where:  
- \(\theta = (\theta_1, \theta_2, \dots, \theta_m)\) are **trainable circuit parameters**  

**PQC layers alternate between:**
- Single-qubit rotations: \(R_x(\theta), R_y(\theta), R_z(\theta)\)  
- Entangling gates: CNOT, CZ → capture feature correlations (e.g., relation between pH and nitrogen)

---

### 2.3 Measurement & Prediction

We measure an observable (usually **Pauli-Z**) on one or more qubits:

\[
y_{\text{pred}} = \langle \psi_{\text{model}} | Z_0 | \psi_{\text{model}} \rangle
\]

This expectation value is converted into **class probabilities**:

- **Binary recommendation (2 crops):**

\[
p(\text{crop}=1) = \frac{1+y_{\text{pred}}}{2}, \quad
p(\text{crop}=0) = 1 - p(\text{crop}=1)
\]

- **Multi-class recommendation:**  
  Multiple qubits are measured → mapped to a **softmax layer** to output probabilities for multiple crops.

---

## 3. Cost Function

We use a **classical loss function** to guide training.  

For **multi-class classification (soil → crop):**

\[
L(\theta) = -\frac{1}{N} \sum_{i=1}^N \sum_{k=1}^K y_{i,k} \log(p_{i,k})
\]

Where:  
- \(y_{i,k} = 1\) if sample \(i\) belongs to crop \(k\), else \(0\)  
- \(p_{i,k} =\) probability predicted by quantum circuit for crop \(k\)  
- \(K =\) number of crop classes  

---

## 4. Optimization

Parameters \(\theta\) are updated **classically** using optimizers like **Adam, COBYLA, SPSA**.  

**Quantum Gradients (Parameter-Shift Rule):**

If  

\[
f(\theta) = \langle \psi(\theta) | \hat{O} | \psi(\theta) \rangle
\]

then the gradient is:

\[
\frac{\partial f}{\partial \theta} =
\frac{f(\theta + \frac{\pi}{2}) - f(\theta - \frac{\pi}{2})}{2}
\]

This enables efficient gradient calculation on quantum hardware.

---

## 5. Why VQC Works for Soil-Crop Recommendation

- **Non-linear separability:**  
  Quantum circuits map soil features into a **high-dimensional Hilbert space**, improving separability.  

- **Entanglement:**  
  Captures correlations between soil properties (e.g., high nitrogen + sandy soil → suitable for maize).  

- **Hybrid design:**  
  Uses classical optimizers with shallow quantum circuits → works on current NISQ devices.  

- **Soil-to-crop mapping:**  
  Directly predicts **optimal crop choice** while implicitly considering soil health and yield.  

---

