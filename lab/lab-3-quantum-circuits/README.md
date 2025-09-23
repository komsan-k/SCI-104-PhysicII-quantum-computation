# Lab 3: Introduction to Quantum Circuits (Google Colab Edition)

## 1. Objectives
By the end of this lab, students will be able to:

- Understand the structure and role of **quantum circuits** in quantum computing.  
- Compose multi-gate, multi-qubit circuits in **Qiskit**.  
- Use **measurement operations** to extract classical outcomes.  
- Simulate circuits with **Qiskit Aer** and interpret results.  
- Visualize circuits, statevectors, and measurement histograms.  

---

## 2. Background

### 2.1 What is a Quantum Circuit?
A **quantum circuit** is a sequence of quantum gates applied to one or more qubits, often followed by measurement into classical bits.  

Mathematically:  
|ψ_out⟩ = U |ψ_in⟩  

### 2.2 Circuit Composition
- **Initialization**: Qubits start in |0⟩.  
- **Gate operations**: Apply single-qubit and multi-qubit gates.  
- **Measurement**: Collapse quantum states to classical outcomes.  

### 2.3 Circuit Visualization
Qiskit provides built-in circuit diagrams (`qc.draw('mpl')`) to display gates and qubit flows.  

### 2.4 Measurement in Circuits
Measurements are probabilistic operations:  
For |ψ⟩ = α|0⟩ + β|1⟩,  
- Probability of 0 = |α|²  
- Probability of 1 = |β|²  

---

## 3. Lab Tasks

### Task 1 — Setup in Google Colab
```python
!pip install qiskit qiskit-aer
from qiskit import QuantumCircuit, transpile
from qiskit_aer import Aer
from qiskit.visualization import plot_histogram
from qiskit.quantum_info import Statevector
```

### Task 2 — Building a Simple Circuit
```python
qc = QuantumCircuit(2,2)
qc.h(0)
qc.cx(0,1)
qc.measure([0,1],[0,1])
qc.draw('mpl')
```

### Task 3 — Simulation with Qiskit Aer
```python
backend = Aer.get_backend('aer_simulator')
tqc = transpile(qc, backend)
result = backend.run(tqc, shots=1024).result()
counts = result.get_counts()
plot_histogram(counts)
```

**Expected outcome:** Only `00` and `11` outcomes, ~50/50.  

### Task 4 — Statevector Simulation
```python
qc_sv = QuantumCircuit(2)
qc_sv.h(0)
qc_sv.cx(0,1)

state = Statevector.from_instruction(qc_sv)
print(state)
```

### Task 5 — Adding More Layers
```python
from math import pi
qc2 = QuantumCircuit(2,2)
qc2.h(0)
qc2.cx(0,1)
qc2.ry(pi/4,0)
qc2.rz(pi/3,1)
qc2.cx(1,0)
qc2.measure([0,1],[0,1])
qc2.draw('mpl')
```

---

## 4. Exercises

1. **Three-Qubit Circuit**: Build a GHZ circuit (|000⟩ → (|000⟩ + |111⟩)/√2).  
2. **Random Circuit**: Generate a 3-qubit circuit with H, X, Z, and CX randomly applied. Explain results.  
3. **Order Matters**: Show that HX ≠ XH by applying gates in different orders on |0⟩.  
4. **Circuit Depth**: Compare depths of equivalent circuits using `qc.depth()`.  
5. **Noise Simulation (Optional)**: Add noise models to observe decoherence.  

---

## 5. Concept Questions

1. What is the difference between a **quantum gate** and a **quantum circuit**?  
2. Why does circuit order matter in quantum computation?  
3. How do **measurements** differ from unitary gates in circuits?  
4. Why is the CNOT gate essential for creating entanglement?  
5. What does circuit **depth** represent and why is it important?  

---

## 6. References
- Qiskit Textbook: [Learn Quantum Computation using Qiskit](https://qiskit.org/textbook/)  
- M. A. Nielsen & I. L. Chuang, *Quantum Computation and Quantum Information*, Cambridge University Press.  
- IBM Quantum Documentation: [https://docs.quantum.ibm.com](https://docs.quantum.ibm.com)  
- J. Preskill, *Quantum Computing Lecture Notes*, Caltech.  

