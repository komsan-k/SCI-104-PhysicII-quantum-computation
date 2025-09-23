# Lab 5: Introduction to Quantum Adders (Google Colab Edition)

## 1. Objectives
By the end of this lab, students will be able to:

- Understand the concept of **quantum addition** and how it differs from classical addition.  
- Implement simple **quantum adder circuits** using Qiskit.  
- Explore **ripple-carry adders** and controlled operations for addition.  
- Simulate small quantum adders and verify correctness.  
- Connect quantum addition to larger algorithms such as modular arithmetic circuits.  

---

## 2. Background

### 2.1 Classical Addition
In classical computing, addition is performed using logic gates (AND, OR, XOR). A full adder combines two input bits and a carry to produce a sum and carry-out.

### 2.2 Quantum Addition
Quantum adders use **reversible logic** — operations must be unitary.  
- Carry operations are built using **CNOT** and **Toffoli (CCX)** gates.  
- Quantum adders preserve information (inputs are not destroyed).  

### 2.3 Types of Quantum Adders
1. **Quantum Ripple-Carry Adder (QRCA):** Quantum equivalent of the classical ripple-carry adder.  
2. **Other optimized adders:** Various designs exist but ripple-carry is the most basic.  

In this lab, we focus on **basic ripple-carry adders**.  

---

## 3. Lab Tasks

### Task 1 — Setup in Google Colab
```python
!pip install qiskit qiskit-aer
from qiskit import QuantumCircuit
from qiskit_aer import Aer
from qiskit.visualization import plot_histogram
```

### Task 2 — Half Adder
```python
qc = QuantumCircuit(3,2)  # a, b, carry

# XOR for sum
qc.cx(0,2)
qc.cx(1,2)

# AND for carry
qc.ccx(0,1,2)

qc.measure([2,0],[0,1])
qc.draw('mpl')
```

### Task 3 — Full Adder
```python
qc = QuantumCircuit(4,2)  # a, b, cin, sum/cout

# Sum = a XOR b XOR cin
qc.cx(0,3)
qc.cx(1,3)
qc.cx(2,3)

# Carry = majority(a,b,cin)
qc.ccx(0,1,2)
qc.ccx(0,1,3)
qc.ccx(1,2,3)

qc.measure([3,2],[0,1])
qc.draw('mpl')
```

### Task 4 — Ripple-Carry Adder
```python
qc = QuantumCircuit(6,4)  # a0,a1,b0,b1,carry,sum

# Example: add a0+b0 with carry
qc.cx(0,4)
qc.cx(2,4)
qc.ccx(0,2,4)

# More gates for higher bits...
qc.measure([0,1,2,3],[0,1,2,3])
qc.draw('mpl')
```

### Task 5 — Simulation
```python
backend = Aer.get_backend('aer_simulator')
result = backend.run(qc, shots=1024).result()
counts = result.get_counts()
plot_histogram(counts)
```

---

## 4. Exercises

1. **Half Adder Check:** Build and simulate the half adder. Verify sum/carry truth table matches the classical case.  
2. **Full Adder Circuit:** Implement a 1-bit full adder with carry-in. Test all possible input combinations.  
3. **2-Bit Ripple-Carry Adder:** Extend to 2 bits. Show measurement results for inputs (01 + 11), (10 + 11).  
4. **3-Bit Ripple-Carry Adder:** Build and test a 3-bit ripple-carry adder. Verify correctness by comparing classical and quantum results.  
5. **Resource Counting:** Count the number of CNOT and Toffoli gates needed for a 2-bit and 3-bit ripple-carry adder. Comment on scalability.  

---

## 5. Concept Questions

1. Why must quantum adders use **reversible logic** instead of standard classical logic?  
2. What roles do **CNOT** and **Toffoli** gates play in building quantum adders?  
3. How does the **carry operation** propagate in a ripple-carry adder?  
4. Why does the size of a ripple-carry adder grow quickly as the number of bits increases?  
5. How could more efficient designs (beyond ripple-carry) improve quantum arithmetic?  

---

## 6. References
- Vedral, V., Barenco, A., & Ekert, A. (1996). *Quantum networks for elementary arithmetic operations.* Physical Review A, 54(1), 147.  
- Cuccaro, S. A., Draper, T. G., Kutin, S. A., & Moulton, D. P. (2004). *A new quantum ripple-carry addition circuit.* arXiv:quant-ph/0410184.  
- Qiskit Textbook: [Learn Quantum Computation using Qiskit](https://qiskit.org/textbook/)  
- Nielsen, M. A. & Chuang, I. L., *Quantum Computation and Quantum Information*, Cambridge University Press.  

---
