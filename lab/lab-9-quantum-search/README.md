# Lab 9: Grover’s Algorithm (Quantum Search) (Google Colab Edition)

## 1. Objectives
By the end of this lab, students will be able to:

- Understand the principle of **Grover’s search algorithm**.  
- Implement Grover’s algorithm in Qiskit for small databases.  
- Explore the role of **oracles** and **diffusion operators**.  
- Analyze how Grover’s algorithm achieves a **quadratic speedup**.  
- Test Grover’s algorithm on toy problems (marked item search).  

---

## 2. Background

### 2.1 Classical Search
- Searching an unsorted database of N items requires O(N) queries.  

### 2.2 Grover’s Algorithm
- Grover reduces search to O(√N) queries.  
- Key components:  
  1. **Oracle**: marks the solution state |x⟩ by flipping its phase.  
  2. **Diffusion Operator**: amplifies amplitude of marked states.  
  3. **Grover Iterations**: repeated oracle + diffusion amplify success probability.  

### 2.3 Applications
- Search problems (databases, optimization).  
- NP-hard problems (SAT, graph problems).  
- Cryptanalysis (e.g., brute force key search).  

---

## 3. Lab Tasks

### Task 1 — Setup in Google Colab
```python
!pip install qiskit qiskit-aer
from qiskit import QuantumCircuit, Aer, transpile
from qiskit.visualization import plot_histogram
```

### Task 2 — Oracle for a 2-Qubit System
Mark state |11⟩.  
```python
oracle = QuantumCircuit(2)
oracle.cz(0,1)  # flip phase if both are 1
oracle.draw('mpl')
```

### Task 3 — Diffusion Operator
```python
def diffusion(n):
    qc = QuantumCircuit(n)
    qc.h(range(n))
    qc.x(range(n))
    qc.h(n-1)
    qc.mct(list(range(n-1)), n-1)  # multi-control Toffoli
    qc.h(n-1)
    qc.x(range(n))
    qc.h(range(n))
    return qc
```

### Task 4 — Grover Iteration
```python
qc = QuantumCircuit(2,2)

# Initial superposition
qc.h([0,1])

# Oracle
qc.append(oracle, [0,1])

# Diffusion
qc.append(diffusion(2), [0,1])

# Measurement
qc.measure([0,1],[0,1])
qc.draw('mpl')
```

### Task 5 — Simulation
```python
backend = Aer.get_backend('aer_simulator')
result = backend.run(transpile(qc, backend), shots=1024).result()
counts = result.get_counts()
plot_histogram(counts)
```

Expected: High probability for |11⟩.  

---

## 4. Exercises

1. **2-Qubit Grover:** Implement Grover’s algorithm to search for |10⟩ instead of |11⟩.  
2. **3-Qubit Grover:** Build oracle + diffusion for a 3-qubit system and mark a chosen state.  
3. **Iteration Count:** Experiment with multiple iterations. What happens if you apply Grover’s iteration too many times?  
4. **Random Oracle:** Choose a random solution in 3-qubit space. Run Grover to find it.  
5. **Scaling:** Discuss why Grover’s quadratic speedup is significant but not exponential.  

---

## 5. Concept Questions

1. What is the role of the **oracle** in Grover’s algorithm?  
2. Why is the **diffusion operator** sometimes called “inversion about the mean”?  
3. How does the number of iterations scale with database size N?  
4. What happens if you run Grover’s algorithm too many times?  
5. Why is Grover’s algorithm important for cryptanalysis (e.g., key search)?  

---

## 6. References
- Lov K. Grover, *A fast quantum mechanical algorithm for database search*, Proceedings, 28th Annual ACM Symposium on Theory of Computing (STOC), 1996.  
- Nielsen, M. A. & Chuang, I. L., *Quantum Computation and Quantum Information*, Cambridge University Press.  
- Qiskit Textbook: [Grover’s Algorithm](https://qiskit.org/textbook/ch-algorithms/grover.html)  
- Preskill, J., *Lecture Notes on Quantum Computation*, Caltech.  

---

