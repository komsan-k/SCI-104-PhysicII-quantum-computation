# Lab 3: Introduction to Quantum Registers (Google Colab Edition)

## 1. Objectives
By the end of this lab, students will be able to:

- Understand the concept of **quantum registers** as containers for qubits.  
- Differentiate between **quantum registers** (qubits) and **classical registers** (measurement outcomes).  
- Construct circuits using explicit register allocation in Qiskit.  
- Map quantum measurements to classical registers.  
- Explore the difference between **register-first** and **circuit-first** approaches in Qiskit.  

---

## 2. Background

### 2.1 Quantum Registers
- A **quantum register** is a collection of qubits used as storage for quantum information.  
- Registers allow us to group qubits and manipulate them collectively.  
- Example: A 2-qubit register can hold states like |00⟩, |01⟩, |10⟩, |11⟩.  

### 2.2 Classical Registers
- A **classical register** stores the results of quantum measurements.  
- They behave like classical memory (bits).  
- Essential for reading quantum outcomes and for feedback in algorithms.  

### 2.3 Registers vs Circuits
- **Quantum register** = where qubits live (storage).  
- **Quantum circuit** = what you do with those qubits (operations).  
- In Qiskit:  
  - You can create **registers first, then circuit**.  
  - Or you can **declare a circuit directly** with qubits and classical bits, and registers are created automatically.  

---

## 3. Lab Tasks

### Task 1 — Setup in Google Colab
```python
!pip install qiskit qiskit-aer
from qiskit import QuantumRegister, ClassicalRegister, QuantumCircuit
from qiskit_aer import Aer
from qiskit.visualization import plot_histogram
```

### Task 2 — Creating Registers Explicitly
```python
qreg = QuantumRegister(2, 'q')   # 2 qubits
creg = ClassicalRegister(2, 'c') # 2 classical bits
qc = QuantumCircuit(qreg, creg)

qc.h(qreg[0])
qc.cx(qreg[0], qreg[1])
qc.measure(qreg, creg)

qc.draw('mpl')
```

### Task 3 — Implicit Register Creation
```python
qc2 = QuantumCircuit(2, 2)  # directly create a circuit with 2 qubits and 2 classical bits
qc2.h(0)
qc2.cx(0,1)
qc2.measure([0,1],[0,1])
qc2.draw('mpl')
```

### Task 4 — Running the Circuit
```python
backend = Aer.get_backend('aer_simulator')
result = backend.run(qc, shots=1024).result()
counts = result.get_counts()
plot_histogram(counts)
```

### Task 5 — Multi-Register Example
```python
qreg1 = QuantumRegister(1, 'a')
qreg2 = QuantumRegister(1, 'b')
creg = ClassicalRegister(2, 'c')

qc3 = QuantumCircuit(qreg1, qreg2, creg)
qc3.h(qreg1[0])
qc3.cx(qreg1[0], qreg2[0])
qc3.measure([qreg1[0], qreg2[0]], [creg[0], creg[1]])
qc3.draw('mpl')
```

---

## 4. Exercises

1. **Register Allocation:** Create a 3-qubit register and apply H to each qubit. What histogram do you get after measurement?  
2. **Multiple Registers:** Create two separate 2-qubit registers and entangle one qubit from each register. Show results.
   <!---
4. **Mapping:** Show what happens if you measure `qreg[0]` into `creg[1]` instead of `creg[0]`.  
5. **Circuit vs Register Style:** Reproduce the same 3-qubit GHZ circuit using (a) explicit registers and (b) direct `QuantumCircuit(3,3)` style. Compare.  
6. **Noise Model (Optional):** Add depolarizing noise to a 2-qubit register and compare histograms.
--->

---

## 5. Concept Questions

1. Why do we need **classical registers** in addition to quantum registers?  
2. What is the difference between creating registers explicitly vs implicitly in Qiskit?  
3. Can a circuit exist without registers? Why or why not?  
4. How does the number of classical bits affect the ability to record measurement results?  
5. In an algorithm like Grover’s, why is it important to manage registers carefully?  

---

## 6. References
- Qiskit Textbook: [Learn Quantum Computation using Qiskit](https://qiskit.org/textbook/)  
- IBM Quantum Documentation: [https://docs.quantum.ibm.com](https://docs.quantum.ibm.com)  
- Nielsen, M. A. & Chuang, I. L., *Quantum Computation and Quantum Information*, Cambridge University Press.  
- Preskill, J., *Quantum Computing Lecture Notes*, Caltech.  

