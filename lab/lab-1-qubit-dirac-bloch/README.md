# Lab 1: Introduction to Qubit, Dirac Notation, and the Bloch Sphere (Google Colab Edition)

## 1. Objectives
By the end of this lab, students will be able to:

- Understand the mathematical representation of a **qubit** in Dirac notation.  
- Translate between Dirac notation and computational basis states.  
- Use **Qiskit in Google Colab** to visualize qubit states.  
- Plot and interpret **Bloch sphere representations**.  
- Explore how quantum gates transform states on the Bloch sphere.  

---

## 2. Background

### 2.1 The Qubit
A qubit is the quantum analogue of a classical bit. While a classical bit can be in state `0` or `1`, a qubit can exist in a **superposition**:

|ψ⟩ = α|0⟩ + β|1⟩,   α, β ∈ C,   |α|² + |β|² = 1

where  

|0⟩ = [1, 0]ᵀ,   |1⟩ = [0, 1]ᵀ

### 2.2 Dirac Notation
- **Ket** (`|ψ⟩`): column vector representation.  
- **Bra** (`⟨ψ|`): conjugate transpose of the ket.  
- Probability of measuring `0`: |α|².  
- Probability of measuring `1`: |β|².  

### 2.3 Bloch Sphere Representation
Any single-qubit pure state can be written as:

|ψ⟩ = cos(θ/2)|0⟩ + e^(iφ)sin(θ/2)|1⟩

This maps to a point on the **Bloch sphere** with coordinates (θ, φ):

- **North pole** (θ=0) → |0⟩.  
- **South pole** (θ=π) → |1⟩.  
- **Equator** → superposition states (e.g., |+⟩ = (|0⟩+|1⟩)/√2).  

---

## 3. Lab Tasks

### Task 1 — Setup in Google Colab
Install Qiskit:
```python
!pip install qiskit qiskit-aer
from qiskit import QuantumCircuit
from qiskit.quantum_info import Statevector
from qiskit.visualization import plot_bloch_multivector
```

### Task 2 — Representing |0⟩ and |1⟩
```python
qc0 = QuantumCircuit(1)
qc1 = QuantumCircuit(1); qc1.x(0)

state0 = Statevector.from_instruction(qc0)
state1 = Statevector.from_instruction(qc1)

plot_bloch_multivector(state0)
plot_bloch_multivector(state1)
```

### Task 3 — Superposition States
```python
qc_plus = QuantumCircuit(1); qc_plus.h(0)
qc_minus = QuantumCircuit(1); qc_minus.x(0); qc_minus.h(0)

state_plus = Statevector.from_instruction(qc_plus)
state_minus = Statevector.from_instruction(qc_minus)

plot_bloch_multivector(state_plus)
plot_bloch_multivector(state_minus)
```

**Expected results:**
- |+⟩: equator (x-axis).  
- |−⟩: opposite equator point (−x-axis).  

### Task 4 — Arbitrary Rotation
```python
from math import pi
qc = QuantumCircuit(1)
qc.ry(pi/3,0)   # Rotate around Y by 60 degrees
state = Statevector.from_instruction(qc)
plot_bloch_multivector(state)
```

---

## 4. Exercises

1. **General State Exploration**: Create a state with θ=π/4, φ=π/2 and plot it.  
2. **Equator States**: Show that θ=π/2 always lies on the equator, independent of φ.  
3. **Phase Factor**: Prove that multiplying |ψ⟩ by a global phase e^(iγ) does not change its Bloch sphere representation. Confirm in Qiskit.  
4. **Rotation Experiment**: Apply rx(pi/2), ry(pi/2), rz(pi/2) to |0⟩ and observe results.  

---

## 5. Concept Questions

1. Geometrically, what does |ψ⟩ = (|0⟩+|1⟩)/√2 represent on the Bloch sphere?  
2. Why must |α|² + |β|² = 1 for a valid qubit state?  
3. What is the difference between a global phase and a relative phase?  
4. Which states lie at the poles of the Bloch sphere?  
5. How do rotation gates Rx, Ry, Rz correspond to movements on the Bloch sphere?  

---

## 6. References
- Qiskit Textbook: [Learn Quantum Computation using Qiskit](https://qiskit.org/textbook/)  
- M. A. Nielsen & I. L. Chuang, *Quantum Computation and Quantum Information*, Cambridge University Press.  
- J. Preskill, *Lecture Notes for Physics 229: Quantum Computation*, Caltech.  
- [IBM Quantum Documentation](https://docs.quantum.ibm.com)  


