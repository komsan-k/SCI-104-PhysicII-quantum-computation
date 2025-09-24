# Lab 8: Shor’s Algorithm (Quantum Factoring) (Google Colab Edition)

## 1. Objectives
By the end of this lab, students will be able to:

- Understand the structure and key steps of **Shor’s algorithm**.  
- Implement a **small-scale factoring example** (e.g., N = 15).  
- Explore the role of **Quantum Phase Estimation (QPE)** in period-finding.  
- Construct **modular exponentiation circuits** in Qiskit.  
- Combine quantum and classical post-processing to recover prime factors.  

---

## 2. Background

### 2.1 Factoring Problem
- Classical factoring of large integers is computationally hard.  
- Security of **RSA cryptography** depends on this hardness.  
- Quantum computers can solve factoring efficiently using Shor’s algorithm.  

### 2.2 Shor’s Algorithm Overview
1. **Classical Preprocessing:** Choose random integer a < N.  
2. **Quantum Order-Finding:**  
   - Compute the period r of function f(x) = a^x mod N.  
   - Use **QPE** + modular exponentiation.  
3. **Classical Postprocessing:**  
   - If r is even, compute gcd(a^(r/2) ± 1, N).  
   - These gcd values yield non-trivial factors of N.  

### 2.3 Why It Works
- Factoring reduces to **order-finding**.  
- QPE allows us to efficiently extract periodicity in the exponent.  
- Classical gcd finds the factors from the period.  

---

## 3. Lab Tasks

### Task 1 — Setup in Google Colab
```python
!pip install qiskit qiskit-aer
from qiskit import QuantumCircuit, Aer, transpile
from qiskit.visualization import plot_histogram
from math import gcd
import numpy as np
```

### Task 2 — Classical Preprocessing
Pick N = 15 (example), and choose random a < N.  
```python
N = 15
a = 7  # random choice, coprime with 15
print("Chosen a =", a)
```

### Task 3 — Modular Exponentiation Circuit
Construct modular exponentiation a^x mod N.  
For small N, Qiskit provides templates:  
```python
from qiskit.circuit.library import ModularExponentiation

modexp = ModularExponentiation(a, 15, 4, 4) # a, N, exponent qubits, modulus qubits
modexp.draw('mpl')
```

### Task 4 — Period Finding with QPE
```python
from qiskit.circuit.library import QFT

n_count = 4  # counting qubits
qc = QuantumCircuit(n_count + 4, n_count)  # 4 qubits for modulus

# Apply Hadamards to counting register
for q in range(n_count):
    qc.h(q)

# Controlled modular exponentiation
for q in range(n_count):
    qc.append(modexp.control(), [q] + list(range(n_count, n_count+4)))

# Inverse QFT
qc.append(QFT(n_count, inverse=True), range(n_count))

# Measurement
qc.measure(range(n_count), range(n_count))

qc.draw('mpl')
```

### Task 5 — Simulation
```python
backend = Aer.get_backend('aer_simulator')
result = backend.run(transpile(qc, backend), shots=2048).result()
counts = result.get_counts()
plot_histogram(counts)
```

### Task 6 — Classical Postprocessing
```python
# Example: suppose we found r = 4
r = 4
factor1 = gcd(a**(r//2) - 1, N)
factor2 = gcd(a**(r//2) + 1, N)
print("Factors of", N, "=", factor1, "and", factor2)
```

---

## 4. Exercises

1. **Factoring 15:** Run Shor’s algorithm for N=15, try different values of a (e.g., 2, 7, 11). Verify factors.  
2. **Factoring 21:** Attempt N=21. What are the results?  
3. **Resource Analysis:** Count number of gates and qubits for N=15 vs N=21.  
4. **Alternative Approach:** Replace modular exponentiation with a simpler circuit for small numbers. Compare.  
5. **Scalability Challenge:** Discuss why factoring RSA-size numbers is not feasible on today’s quantum devices.  

---

## 5. Concept Questions

1. Why is factoring reduced to **order-finding** in Shor’s algorithm?  
2. How does QPE extract the period of modular exponentiation?  
3. Why must r be **even** for the gcd step to yield factors?  
4. What role does **classical computation** play in Shor’s algorithm?  
5. Why is Shor’s algorithm considered a threat to classical cryptography?  

---

## 6. References
- P. W. Shor, *Algorithms for quantum computation: discrete logarithms and factoring*, Proc. 35th IEEE Symposium on Foundations of Computer Science, 1994.  
- Nielsen, M. A. & Chuang, I. L., *Quantum Computation and Quantum Information*, Cambridge University Press.  
- Qiskit Textbook: [Shor’s Algorithm](https://qiskit.org/textbook/ch-algorithms/shor.html)  
- Preskill, J., *Lecture Notes on Quantum Computation*, Caltech.  

---

