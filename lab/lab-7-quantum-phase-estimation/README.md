# Lab 7: Introduction to Quantum Phase Estimation (QPE) (Google Colab Edition)

## 1. Objectives
By the end of this lab, students will be able to:

- Understand the principle of **Quantum Phase Estimation (QPE)**.  
- Implement QPE circuits in Qiskit using **controlled unitaries** and **inverse QFT**.  
- Simulate QPE on small unitaries (e.g., Pauli gates).  
- Analyze the trade-off between **precision (number of qubits)** and **accuracy**.  
- Connect QPE to major algorithms such as **Shor’s factoring** and **quantum chemistry** (eigenvalue estimation).  

---

## 2. Background

### 2.1 Problem Statement
Given:  
- A unitary operator U.  
- An eigenvector |ψ⟩ such that:  
U|ψ⟩ = e^(2πiϕ)|ψ⟩  
where ϕ is an unknown phase.  

**Goal:** Estimate the phase ϕ to n-bit precision.  

### 2.2 Algorithm Overview
1. Prepare **two registers**:  
   - **Control register (n qubits):** to encode the phase.  
   - **Target register (m qubits):** initialized to an eigenstate |ψ⟩.  
2. Apply **Hadamard gates** to control register.  
3. Apply **controlled-U^(2^j)** gates, entangling phase with control qubits.  
4. Apply **inverse QFT** on the control register.  
5. Measure control register → binary approximation of ϕ.  

### 2.3 Applications
- **Shor’s Algorithm:** Finds the order of integers modulo N (period finding).  
- **Quantum Chemistry:** Computes eigenvalues of Hamiltonians.  
- **Quantum Simulation:** Extracts energy spectra of physical systems.  

---

## 3. Lab Tasks

### Task 1 — Setup in Google Colab
```python
!pip install qiskit qiskit-aer
from qiskit import QuantumCircuit, Aer, transpile
from qiskit.visualization import plot_histogram
import numpy as np
```

### Task 2 — Controlled Unitary Example
Example: U = Z (Pauli-Z gate).  
Z|0⟩ = |0⟩,   Z|1⟩ = -|1⟩  
Eigenvalue of |1⟩ has phase ϕ = 0.5.  

```python
qc = QuantumCircuit(2,1)
qc.h(0)          # Hadamard on control
qc.cx(0,1)       # Controlled-Z equivalent
qc.h(0)
qc.measure(0,0)
qc.draw('mpl')
```

### Task 3 — QPE Circuit Template
```python
def qpe(unitary, n_count):
    qc = QuantumCircuit(n_count + 1, n_count)
    
    # Apply Hadamards to counting qubits
    for q in range(n_count):
        qc.h(q)
    
    # Controlled-U^{2^j}
    for q in range(n_count):
        qc.append(unitary.control(), [q, n_count])
    
    # Inverse QFT
    from qiskit.circuit.library import QFT
    qc.append(QFT(num_qubits=n_count, inverse=True), range(n_count))
    
    # Measure
    qc.measure(range(n_count), range(n_count))
    return qc
```

### Task 4 — Run QPE on Pauli-Z
```python
from qiskit.quantum_info import Operator
from qiskit.circuit.library import ZGate

qc = qpe(ZGate(), 3)  # 3 counting qubits
backend = Aer.get_backend('aer_simulator')
result = backend.run(transpile(qc, backend), shots=2048).result()
counts = result.get_counts()
plot_histogram(counts)
```

Expected: Peak around binary fraction `100` (≈0.5).  

### Task 5 — Testing with Other Unitaries
Try:  
- Pauli-X  
- Phase gate S (e^(iπ/2))  
- T gate (e^(iπ/4))  

Compare measured phases with theoretical values.  

---

## 4. Exercises

1. **Single Qubit QPE:** Implement QPE with 1 counting qubit for the Z gate. What phase precision can you achieve?  
2. **Scaling:** Increase counting qubits from 2 to 5 for the T gate. How does precision improve?  
3. **Custom Controlled Unitary:** Create a controlled-U^2 operation manually for U=X. Verify phase results.  
4. **Inverse QFT Check:** Replace inverse QFT with identity. What happens to results?  
5. **Noise Model (Optional):** Add depolarizing noise and observe how QPE results degrade.  

---

## 5. Concept Questions

1. Why does QPE require an **inverse QFT** on the counting register?  
2. How does the number of counting qubits affect phase accuracy?  
3. What role do **controlled-U** gates play in QPE?  
4. Why is QPE considered a building block of Shor’s algorithm?  
5. How could errors in eigenstate preparation affect QPE outcomes?  

---

## 6. References
- Nielsen, M. A. & Chuang, I. L., *Quantum Computation and Quantum Information*, Cambridge University Press.  
- Kitaev, A. Y. (1995). *Quantum measurements and the Abelian Stabilizer Problem.* arXiv:quant-ph/9511026.  
- Qiskit Textbook: [Learn Quantum Computation using Qiskit](https://qiskit.org/textbook/)  
- Cleve, R., Ekert, A., Macchiavello, C., & Mosca, M. (1998). *Quantum algorithms revisited.* Proc. R. Soc. Lond. A.  

---

