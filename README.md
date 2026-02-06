# BenchQC

quantumBENCH is a Python package for benchmarking quantum computers and their performance of the Variational Quantum Eigensolver (VQE) on chemical systems. This package aims to contribute to materials discovery and design by providing tools for quantum chemistry simulations.

## Features

- Generate qubit operators from molecular data
- Compute energies using VQE and classical solvers
- Supports various quantum devices and optimizers

## Installation

You can install the package using `pip`. First, clone the repository:

```bash
git clone https://github.com/niapollard/quantumBENCH.git
cd quantumBENCH
```

Then install the package:

```bash
pip install .
```
You can find installation examples in Google Colab notebooks below

<a name="example"></a>
Examples
---------


| Notebooks                                                                                                                                      | Google&nbsp;Colab                                                                                                                                        | Descriptions                                                                                                                                                                                                                                                                                                                                                                                              |
| ---------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [Cube File Generation](https://colab.research.google.com/github/niapollard/Benchmarking-Quantum-Algorithms-for-Materials-Discovery-and-Design/blob/main/Cube_File_Generation.ipynb)                                                       | [![Open in Google Colab]](https://colab.research.google.com/github/niapollard/Benchmarking-Quantum-Algorithms-for-Materials-Discovery-and-Design/blob/main/Cube_File_Generation.ipynb)                                 | Example of cube file generation for electronic denisty visualization..                                                                                                                                                                                                                                                                       |
| [Bond Distance Plot](https://colab.research.google.com/github/niapollard/Benchmarking-Quantum-Algorithms-for-Materials-Discovery-and-Design/blob/main/Al2_Bond_Distance_Plot_nm.ipynb)                                                  | [![Open in Google Colab]](https://colab.research.google.com/github/niapollard/Benchmarking-Quantum-Algorithms-for-Materials-Discovery-and-Design/blob/main/Al2_Bond_Distance_Plot_nm.ipynb)                            | Example of varying the bond distance between two Aluminum atoms with plot.                                                                                                                                                                                                                                                                                                                                 |
| [Classical Optimizer Variation](https://colab.research.google.com/github/niapollard/Benchmarking_QC/blob/main/Varying_Classical_Optimizers_PLOT_Correct_version.ipynb)                                                  | [![Open in Google Colab]](https://colab.research.google.com/github/niapollard/Benchmarking_QC/blob/main/Varying_Classical_Optimizers_PLOT_Correct_version.ipynb)                          | Example of obtaining VQE energies with varied classical optimizers.                                                                                                                                                                                                                                                                                                                                 |
| [Quantum Circuit Variation](https://colab.research.google.com/github/niapollard/Benchmarking-Quantum-Algorithms-for-Materials-Discovery-and-Design/blob/main/Al_Varies_Circuit.ipynb)                                                 | [![Open in Google Colab]](https://colab.research.google.com/github/niapollard/Benchmarking-Quantum-Algorithms-for-Materials-Discovery-and-Design/blob/main/Al_Varies_Circuit.ipynb)                           | Example of obtaining VQE energies with varied quantum circuit type (circuits obtained from Jarvis-tools).                                                                                                                                                                                                                                             
| [Varying Number of Repetitions](https://colab.research.google.com/github/niapollard/Benchmarking-Quantum-Algorithms-for-Materials-Discovery-and-Design/blob/main/Table%201/S7%3A%20Varies%20Reps.ipynb)                  | [![Open in Google Colab]](https://colab.research.google.com/github/niapollard/Benchmarking-Quantum-Algorithms-for-Materials-Discovery-and-Design/blob/main/Table%201/S7%3A%20Varies%20Reps.ipynb)  | Example of obtaining VQE energies with varied number of quantum circuit repetitions. |
| [Quantum Simulator Variation](https://colab.research.google.com/github/niapollard/Benchmarking_QC/blob/main/Varying%20Simulator.ipynb)                                                  | [![Open in Google Colab]](https://colab.research.google.com/github/niapollard/Benchmarking_QC/blob/main/Varying%20Simulator.ipynb)                          | Example of obtaining VQE energies with varied quantum simulator type.                                                                                                                                                                                                                                                                                                                                 |
| [Varying Basis Sets](https://colab.research.google.com/github/niapollard/Benchmarking_QC/blob/main/052024_Varying%20Basis%20Set.ipynb)                   | [![Open in Google Colab]](https://colab.research.google.com/github/niapollard/Benchmarking_QC/blob/main/052024_Varying%20Basis%20Set.ipynb)  | Example of obtaining VQE energies with varied basis set type. |
                                                                                                     

[Open in Google Colab]: https://colab.research.google.com/assets/colab-badge.svg

## Usage

Here's an example of how to use the package:

```python
from quantum_vqe import get_qubit_op, get_energy
from qiskit.algorithms.optimizers import SLSQP
from qiskit.providers.aer import AerSimulator
from qiskit_nature.drivers import Molecule
from qiskit_nature.mappers.second_quantization import JordanWignerMapper

# Define a molecule
molecule = Molecule(geometry=[['H', [0., 0., 0.]], ['H', [0., 0., 0.735]]], multiplicity=1, charge=0)

# Get the qubit operator
res1 = get_qubit_op(molecule)

# Define an optimizer and quantum device
optimizer = SLSQP(maxiter=1000)
device = Aer.get_backend('statevector_simulator')

# Compute the energy
res = get_energy(optimizer, device, res1['qubit_op'])

print("Computed Energy:", res['eigenvalue'])
```

## API Reference

### `get_qubit_op`

```python
get_qubit_op(molecule, basis='sto3g', functional='lda', method=MethodType.RKS, driver_type=ElectronicStructureDriverType.PYSCF, mapper=JordanWignerMapper())
```

- `molecule`: The molecule to be simulated.
- `basis`: The basis set to be used (default: 'sto3g').
- `functional`: The functional to be used (default: 'lda').
- `method`: The method to be used (default: `MethodType.RKS`).
- `driver_type`: The electronic structure driver type (default: `ElectronicStructureDriverType.PYSCF`).
- `mapper`: The mapper to convert to qubit operator (default: `JordanWignerMapper`).

Returns a dictionary with the keys:
- `qubit_op`: The qubit operator.
- `converter`: The qubit converter.
- `problem_reduced`: The reduced electronic structure problem.
- `numpy_solver`: The numpy solver.

### `get_energy`

```python
get_energy(optimizer, device, qubit_op, seed=42, reps=1)
```

- `optimizer`: The optimizer to be used.
- `device`: The quantum device to be used.
- `qubit_op`: The qubit operator.
- `seed`: The seed for the random number generator (default: 42).
- `reps`: The number of repetitions (default: 1).

Returns a dictionary with the keys:
- `eigenvalue`: The computed eigenvalue.
- `vqe`: The VQE instance.
- `qi`: The quantum instance.

## Contributing

Contributions are welcome! Please submit a pull request or open an issue to discuss changes.

## License

This project is licensed under the NIST License - see the [LICENSE](LICENSE) file for details.

## Acknowledgments

- This package uses [Qiskit](https://qiskit.org/), an open-source SDK for working with quantum computers.

## Contact

For more information, please contact [Nia Pollard](mailto:nia.rodney-pollard@nist.gov).

