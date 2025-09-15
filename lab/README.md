# Introduction to Qiskit with Google Colab

This guide introduces how to use **Qiskit** in **Google Colab**, a
cloud-based Jupyter notebook environment, for learning and experimenting
with quantum computing.

------------------------------------------------------------------------

## 1.1 Why Use Qiskit in Google Colab?

Google Colab is ideal for Qiskit because: - ✅ No setup required: just
install Qiskit with `pip install`.\
- ✅ GPU/TPU not necessary (quantum circuits run on simulators or IBM
Quantum cloud).\
- ✅ Easy to share notebooks with students, collaborators, or
researchers.

------------------------------------------------------------------------

## 1.2 Setting Up Qiskit in Colab

In a new Colab notebook, run:

``` python
!pip install qiskit qiskit_ibm_runtime matplotlib
```

This installs **Qiskit**, **IBM Runtime**, and **Matplotlib** for
visualization.

------------------------------------------------------------------------

## 1.3 First Circuit in Qiskit (Colab)

``` python
from qiskit import QuantumCircuit, Aer, execute
from qiskit.visualization import plot_histogram
import matplotlib.pyplot as plt

# Create a simple 1-qubit circuit
qc = QuantumCircuit(1, 1)

# Apply Hadamard (superposition)
qc.h(0)

# Measure
qc.measure(0, 0)

# Draw circuit
print(qc.draw())

# Run on simulator
simulator = Aer.get_backend('qasm_simulator')
result = execute(qc, simulator, shots=1000).result()
counts = result.get_counts()

# Plot results
plot_histogram(counts)
plt.show()
```

📌 **Colab Tip**: Visualizations (histograms, Bloch spheres) render
inline by default.

------------------------------------------------------------------------

## 1.4 Visualizing States on the Bloch Sphere

``` python
from qiskit.quantum_info import Statevector
from qiskit.visualization import plot_bloch_multivector

# Initial |0⟩ state
qc0 = QuantumCircuit(1)
state0 = Statevector.from_instruction(qc0)
plot_bloch_multivector(state0)

# After Hadamard
qc1 = QuantumCircuit(1)
qc1.h(0)
state1 = Statevector.from_instruction(qc1)
plot_bloch_multivector(state1)
```

-   \|0⟩ appears at the **north pole**.\
-   After Hadamard → on the **equator**, a balanced superposition.

------------------------------------------------------------------------

## 1.5 Running on IBM Quantum Hardware from Colab

1.  Create an IBM Quantum account.\
2.  Get your API token under **My Account**.\
3.  Save and load it in Colab:

``` python
from qiskit_ibm_runtime import QiskitRuntimeService

# Save your token (only once per session)
QiskitRuntimeService.save_account(channel="ibm_quantum", token="YOUR_API_TOKEN_HERE", overwrite=True)

# Load service
service = QiskitRuntimeService()

# List available backends
service.backends()
```

Run a circuit on IBM Quantum:

``` python
# Example: run our first circuit on IBM simulator
backend = service.backend("ibmq_qasm_simulator")

job = service.run(program_id="circuit-runner",
                  options={"backend_name": backend.name},
                  inputs={"circuit": qc})
result = job.result()
print(result)
```

📌 **Note**: Real hardware runs may queue for minutes. Simulators are
faster for learning.

------------------------------------------------------------------------

## 1.6 Lab Work (Colab-Friendly)

### Lab 1: Superposition Experiment

-   Apply Hadamard to \|0⟩.\
-   Run **1000 shots**.\
-   Compare simulator vs IBM Quantum results.

``` python
job = service.run(program_id="circuit-runner",
                  options={"backend_name": "ibmq_qasm_simulator"},
                  inputs={"circuit": qc})
print(job.result())
```

------------------------------------------------------------------------

### Lab 2: Entanglement in Colab

Create a **Bell State**:

``` python
qc2 = QuantumCircuit(2, 2)
qc2.h(0)
qc2.cx(0, 1)
qc2.measure([0,1], [0,1])

print(qc2.draw())

# Run simulation
result2 = execute(qc2, simulator, shots=1000).result()
counts2 = result2.get_counts()

plot_histogram(counts2)
plt.show()
```

Expected results: \~50% `00`, \~50% `11`.

------------------------------------------------------------------------

### Lab 3: Saving Results to Google Drive

``` python
from google.colab import drive
drive.mount('/content/drive')

# Save histogram
plot_histogram(counts2).savefig('/content/drive/MyDrive/quantum_results.png')
```

Your results will now be stored in Google Drive.

------------------------------------------------------------------------

## 1.7 Summary

**Qiskit + Colab** = zero setup, easy sharing.

You learned how to: - Install Qiskit in Colab.\
- Run simple circuits.\
- Visualize results (histogram, Bloch sphere).\
- Connect to IBM Quantum hardware.\
- Save outputs to Google Drive.

👉 This workflow makes Colab an excellent **classroom** and **research**
tool for quantum computing.

