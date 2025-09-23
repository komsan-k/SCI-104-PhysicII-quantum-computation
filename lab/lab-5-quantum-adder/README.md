# Lab 5: Introduction to Quantum Adders (Google Colab Edition)

## 1. Objectives
By the end of this lab, students will be able to:

- Understand the concept of **quantum addition** and how it differs from classical addition.  
- Implement simple **quantum adder circuits** using Qiskit.  
- Explore **ripple-carry adders** and controlled operations for addition.  
- Simulate small quantum adders and verify correctness.  
- Connect quantum addition to larger algorithms (e.g., Shor’s factoring algorithm).  

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
2. **Quantum Fourier Transform (QFT)-based Adder:** Uses QFT to perform addition in the Fourier basis (efficient for large numbers).  

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

1. **Half Adder Check:** Build and simulate the half adder. Verify sum/carry truth table matches classical case.  
2. **Full Adder Circuit:** Implement a 1-bit full adder with carry-in. Test all input combinations.  
3. **2-Bit Ripple-Carry Adder:** Extend to 2 bits. Show measurement results for inputs (01 + 11), (10 + 11).  
4. **QFT Adder Exploration (Optional):** Research and implement a 2-qubit QFT adder in Qiskit.  
5. **Resource Counting:** Compare the number of gates in ripple-carry vs QFT adder. Which is more efficient for large n?  

---

## 5. Concept Questions

1. Why must quantum adders use reversible logic?  
2. What roles do CNOT and Toffoli gates play in quantum adders?  
3. Why is the ripple-carry adder inefficient for large numbers?  
4. How can the QFT-based adder improve efficiency?  
5. In which quantum algorithm is a QFT adder especially useful?  

---

## 6. References
- Vedral, V., Barenco, A., & Ekert, A. (1996). *Quantum networks for elementary arithmetic operations.* Physical Review A, 54(1), 147.  
- Cuccaro, S. A., Draper, T. G., Kutin, S. A., & Moulton, D. P. (2004). *A new quantum ripple-carry addition circuit.* arXiv:quant-ph/0410184.  
- Qiskit Textbook: [Learn Quantum Computation using Qiskit](https://qiskit.org/textbook/)  
- Nielsen, M. A. & Chuang, I. L., *Quantum Computation and Quantum Information*, Cambridge University Press.  

---

