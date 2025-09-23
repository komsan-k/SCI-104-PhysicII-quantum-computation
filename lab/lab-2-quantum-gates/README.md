# Lab 2: Introduction to Quantum Gates (Google Colab Edition)

## 1. Objectives
By the end of this lab, students will be able to:

- Understand the role of quantum gates as unitary operators.  
- Implement basic **single-qubit gates** (X, Y, Z, H, S, T, rotation gates).  
- Explore **multi-qubit gates** (CNOT, CZ, SWAP).  
- Visualize quantum gate effects on circuits and Bloch spheres.  
- Interpret how quantum gates build complex operations from simple primitives.  

---

## 2. Background

### 2.1 Quantum Gates
- Quantum gates are **unitary matrices** acting on qubits.  
- They preserve normalization (U†U = I).  
- They transform states according to:  

|ψ'⟩ = U |ψ⟩  

### 2.2 Common Single-Qubit Gates
- **Pauli-X (NOT):** Flips |0⟩ ↔ |1⟩.  
- **Pauli-Y:** Rotation around Y-axis with phase.  
- **Pauli-Z:** Phase flip on |1⟩.  
- **Hadamard (H):** Creates superposition.  
- **Phase (S, T):** Adds phase factors.  
- **Rotation gates (Rx, Ry, Rz):** Continuous rotations around axes.  

### 2.3 Multi-Qubit Gates
- **CNOT (CX):** Flips target if control = 1.  
- **CZ:** Applies Z to target if control = 1.  
- **SWAP:** Exchanges two qubits.  

### 2.4 Universality
Any quantum circuit can be built from a **universal set** of gates: {H, T, CNOT}.  

---

## 3. Lab Tasks

### Task 1 — Setup in Google Colab
```python
!pip install qiskit qiskit-aer
from qiskit import QuantumCircuit
from qiskit.visualization import plot_bloch_multivector, plot_histogram
from qiskit.quantum_info import Statevector
from qiskit_aer import Aer
```

### Task 2 — Pauli Gates
```python
qc = QuantumCircuit(1)
qc.x(0)  # Try qc.y(0), qc.z(0)
state = Statevector.from_instruction(qc)
plot_bloch_multivector(state)
```

### Task 3 — Hadamard Gate
```python
qc = QuantumCircuit(1,1)
qc.h(0)
qc.measure(0,0)

backend = Aer.get_backend('aer_simulator')
result = backend.run(qc, shots=1024).result()
plot_histogram(result.get_counts())
```

**Expected outcome:** ~50/50 measurement results.  

### Task 4 — Phase and Rotation Gates
```python
qc = QuantumCircuit(1)
qc.s(0)   # try qc.t(0)
qc.h(0)
state = Statevector.from_instruction(qc)
plot_bloch_multivector(state)
```

```python
from math import pi
qc = QuantumCircuit(1)
qc.ry(pi/4,0)
state = Statevector.from_instruction(qc)
plot_bloch_multivector(state)
```

### Task 5 — Two-Qubit Gates
```python
bell = QuantumCircuit(2,2)
bell.h(0)
bell.cx(0,1)
bell.measure([0,1],[0,1])

backend = Aer.get_backend('aer_simulator')
result = backend.run(bell, shots=1024).result()
plot_histogram(result.get_counts())
```

---

## 4. Exercises

1. **X-Y-Z Rotation**: Apply Rx(π/2), Ry(π/2), Rz(π/2) to |0⟩ and plot results.  
2. **H-S Combination**: Apply HS to |0⟩, visualize the state.  
3. **SWAP Gate**: Prepare |01⟩, apply SWAP, confirm |10⟩.  
4. **CZ Gate**: Prepare superposition on control qubit, apply CZ, analyze effect.  
5. **Universality Challenge**: Show how {H, T, CNOT} can approximate other gates.  

---

## 5. Concept Questions

1. Why must quantum gates be **unitary**?  
2. What is the geometric interpretation of Pauli-X, Y, Z on the Bloch sphere?  
3. How does the Hadamard gate relate to measurement probabilities?  
4. What makes {H, T, CNOT} a universal gate set?  
5. Why can’t measurement be represented as a unitary operation?  

---

## 6. References
- Qiskit Textbook: [Learn Quantum Computation using Qiskit](https://qiskit.org/textbook/)  
- M. A. Nielsen & I. L. Chuang, *Quantum Computation and Quantum Information*, Cambridge University Press.  
- M. Preskill, *Quantum Computing Lecture Notes*, Caltech.  
- [IBM Quantum Documentation](https://docs.quantum.ibm.com)  

