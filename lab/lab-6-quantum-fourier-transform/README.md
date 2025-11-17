# Lab 6: Introduction to Quantum Fourier Transform (QFT) (Google Colab Edition)

## 1. Objectives
By the end of this lab, students will be able to:

- Understand the concept of the **Quantum Fourier Transform (QFT)** and how it relates to the classical Discrete Fourier Transform (DFT).  
- Implement QFT circuits in Qiskit using Hadamard and controlled-phase gates.  
- Explore how QFT can be used for **arithmetic operations** and **period finding**.  
- Simulate small QFT circuits and interpret their results.  
- Connect QFT to important quantum algorithms (Phase Estimation, Shor’s algorithm).  

---

## 2. Background

### 2.1 Classical Fourier Transform
The **Discrete Fourier Transform (DFT)** maps a vector of amplitudes (x₀, x₁, …, xₙ₋₁) to frequency space:

$$yₖ = (1/√N) Σⱼ xⱼ e^(2πi jk / N)$$

### 2.2 Quantum Fourier Transform
The QFT is the quantum analogue of the DFT.  
- It acts on computational basis states:  
$$|j⟩ → (1/√N) Σₖ e^(2πi jk / N) |k⟩$$  
- Implemented with **Hadamard gates** and **controlled phase rotations**.  
- Requires only O(n²) gates for an n-qubit register, which is exponentially faster than classical FFT on the statevector.  

### 2.3 Applications
- **Quantum Phase Estimation (QPE)**  
- **Shor’s Factoring Algorithm**  
- **Quantum arithmetic** (efficient adders and modular operations)  

---

## 3. Lab Tasks

### Task 1 — Setup in Google Colab
```python
!pip install qiskit qiskit-aer
from qiskit import QuantumCircuit
from qiskit_aer import Aer
from qiskit.visualization import plot_histogram
import numpy as np
```

### Task 2 — QFT Building Block
```python
def qft(n):
    qc = QuantumCircuit(n)
    for j in range(n):
        qc.h(j)
        for k in range(j+1, n):
            qc.cp(np.pi/2**(k-j), k, j)
    # Swap qubits for final ordering
    for j in range(n//2):
        qc.swap(j, n-j-1)
    return qc
```

### Task 3 — 2-Qubit QFT
```python
qc = qft(2)
qc.draw('mpl')
```

### Task 4 — Testing QFT
```python
from qiskit.quantum_info import Statevector

for i in range(4):
    qc_test = QuantumCircuit(2)
    if i & 1: qc_test.x(0)
    if i & 2: qc_test.x(1)
    qc_test.append(qft(2), [0,1])
    state = Statevector.from_instruction(qc_test)
    print(f"Input |{i:02b}>, Output state = {state}")

print(f"Input |{i:02b}>, Output state = {state}")
plot_bloch_multivector(state)
```
<!---
### Task 5 — Inverse QFT
```python
qft_circ = qft(3)
inv_qft_circ = qft_circ.inverse()
inv_qft_circ.draw('mpl')
```

### Task 6 — QFT in Arithmetic
- Encode a number in a register.  
- Apply QFT.  
- Apply phase rotations proportional to the number to add.  
- Apply inverse QFT.  

---
--->
## 4. Exercises

1. **Manual QFT(3):** Write the 3-qubit QFT circuit by hand using H and controlled-phase gates. Compare with the function output.  
2. **Inverse Check:** Apply QFT followed by its inverse. Verify the state returns to input.  
3. **QFT Addition:** Implement a 2-qubit QFT adder to add +1 to any input. Verify outputs.  
4. **Scaling:** Try QFT on 4 qubits. How many controlled-phase gates are required?  
5. **Efficiency Comparison:** Compare the circuit depth of a 3-qubit ripple-carry adder vs a 3-qubit QFT adder.  

---

## 5. Concept Questions

1. Why is QFT exponentially faster than the classical DFT?  
2. Why do we need to **swap qubits** at the end of QFT?  
3. What is the role of controlled-phase gates in QFT?  
4. Why is QFT central to Shor’s algorithm?  
5. What happens if you omit the final swaps in QFT?  

---

## 6. References
- Nielsen, M. A. & Chuang, I. L., *Quantum Computation and Quantum Information*, Cambridge University Press.  
- Vedral, V., Barenco, A., & Ekert, A. (1996). *Quantum networks for elementary arithmetic operations.* Physical Review A, 54(1), 147.  
- Coppersmith, D. (2002). *An approximate Fourier transform useful in quantum factoring.* arXiv:quant-ph/0201067.  
- Qiskit Textbook: [Learn Quantum Computation using Qiskit](https://qiskit.org/textbook/)  

