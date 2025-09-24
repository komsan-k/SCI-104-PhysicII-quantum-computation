# Lab 10: Quantum Error Correction (QEC) (Google Colab Edition)

## 1. Objectives
By the end of this lab, students will be able to:

- Understand why **quantum error correction** is necessary.  
- Identify **common error types** (bit-flip, phase-flip, depolarizing).  
- Implement a **3-qubit repetition code** for bit-flip error correction.  
- Extend concepts to **phase-flip correction** and **Shor code**.  
- Explore trade-offs between **physical qubits** and **logical qubits**.  

---

## 2. Background

### 2.1 The Problem of Quantum Errors
- Qubits are fragile → prone to:  
  - **Bit-flip errors (X):** |0⟩ → |1⟩.  
  - **Phase-flip errors (Z):** |+⟩ → |−⟩.  
  - **Depolarizing noise:** random error with some probability.  
- Unlike classical bits, we **cannot clone** quantum states → direct redundancy is not allowed.  

### 2.2 Error Correction Idea
- Encode **1 logical qubit** into **multiple physical qubits**.  
- Measure **syndrome qubits** (ancillas) to detect errors.  
- Apply correction gates conditioned on syndrome.  

### 2.3 Simple Codes
- **3-Qubit Repetition Code:** protects against 1 bit-flip error.  
- **Phase-Flip Code:** similar, but in Hadamard basis.  
- **Shor’s 9-Qubit Code:** combines both.  

---

## 3. Lab Tasks

### Task 1 — Setup in Google Colab
```python
!pip install qiskit qiskit-aer
from qiskit import QuantumCircuit, Aer, transpile
from qiskit.visualization import plot_histogram
import numpy as np
```

### Task 2 — 3-Qubit Encoding
Encode |ψ⟩ = α|0⟩ + β|1⟩ into 3 qubits.  
```python
qc = QuantumCircuit(3,1)

# Encode: |q0> → |q0,q1,q2>
qc.cx(0,1)
qc.cx(0,2)

qc.draw('mpl')
```

### Task 3 — Inject a Bit-Flip Error
```python
# Introduce an error (X on qubit 1)
qc.x(1)
qc.draw('mpl')
```

### Task 4 — Majority Voting (Decoding)
```python
# Decode
qc.cx(0,1)
qc.cx(0,2)
qc.ccx(1,2,0)   # majority correction

qc.measure(0,0)
qc.draw('mpl')
```

### Task 5 — Simulation
```python
backend = Aer.get_backend('aer_simulator')
result = backend.run(transpile(qc, backend), shots=1024).result()
counts = result.get_counts()
plot_histogram(counts)
```

Expected: State is corrected back to original.  

---

## 4. Exercises

1. **Bit-Flip Correction:** Show that the 3-qubit repetition code corrects 1 bit-flip error but fails if 2 qubits flip.  
2. **Phase-Flip Code:** Implement a 3-qubit phase-flip code using Hadamard gates. Test with a Z error.  
3. **Combined Code:** Research Shor’s 9-qubit code and explain how it protects against both X and Z errors.  
4. **Noise Simulation:** Use Qiskit’s `noise` module to simulate depolarizing noise and test the error correction circuit.  
5. **Overhead Analysis:** Calculate number of physical qubits needed to encode 1 logical qubit with the 3-qubit and 9-qubit codes.  

---

## 5. Concept Questions

1. Why can’t we just “copy” a qubit state to protect it from errors?  
2. How does the 3-qubit repetition code detect and correct errors?  
3. Why do we need both **bit-flip** and **phase-flip** protection?  
4. What is the trade-off between **logical** and **physical** qubits in QEC?  
5. How does QEC enable **fault-tolerant quantum computation**?  

---

## 6. References
- Nielsen, M. A. & Chuang, I. L., *Quantum Computation and Quantum Information*, Cambridge University Press.  
- Shor, P. W. (1995). *Scheme for reducing decoherence in quantum computer memory*. Phys. Rev. A.  
- Qiskit Textbook: [Quantum Error Correction](https://qiskit.org/textbook/ch-quantum-hardware/error-correction-repetition-code.html)  
- Preskill, J., *Quantum Computing in the NISQ era and beyond*, Quantum 2, 79 (2018).  

---

