---

title: "How Strangeworks Uses Amazon Braket to Explore Aircraft Loading Optimization"
date: 2025-09-02
weight: 3
chapter: false
pre: "<b>3. </b>"
-----------------

{{% notice info %}}
Original article published on the [AWS Quantum Technologies Blog](https://aws.amazon.com/blogs/quantum-computing/)
{{% /notice %}}

Quantum computing promises breakthroughs for solving complex problems across industries, although it remains an open question how practically useful this technology will be. In this blog, the team from Strangeworks, an AWS partner, evaluates different implementations of the **QAOA (Quantum Approximate Optimization Algorithm)** on the aircraft loading optimization problem posed by Airbus as part of last year’s [Quantum Mobility Challenge](https://qcc.thequantuminsider.com/).

## Overview of the Problem

The aircraft loading problem presented by Airbus is a type of **bin packing** optimization, common in many industries such as travel, manufacturing, and logistics. These problems are hard to solve because the number of possibilities grows exponentially as variables increase, creating a huge solution space even for relatively small instances.

### Why Use Quantum Computing?

Quantum computers may be well-suited to solving these optimization problems, potentially offering speedups compared to classical approaches. However, since current quantum hardware is not yet superior to the best classical methods, hybrid approaches that combine quantum and classical processing are under active consideration.

{{% notice note %}}
Current gate-based quantum hardware has not yet outperformed top classical technologies, but the benchmarking approach in this article achieves accurate results for problems up to **80 qubits/variables**.
{{% /notice %}}

## QAOA Algorithms Used

QAOA is a **hybrid quantum-classical** algorithm, meaning it leverages both quantum and classical computation, and is considered well-suited for the NISQ (noisy intermediate-scale quantum) era. The Strangeworks team based their work on three QAOA variants:

1. **Standard QAOA**: Available in many open-source libraries, including the [Braket algorithm library](https://aws.amazon.com/blogs/quantum-computing/introducing-the-amazon-braket-algorithm-library/)

2. **Relax-and-Round QAOA (QRR)**: A variant developed by the Rigetti Computing team

3. **StrangeworksQAOA**: A proprietary variant with improvements in classical processing, outperforming standard QAOA for this problem

## Using Braket Hybrid Jobs

Combining quantum and classical resources through hybrid algorithms like QAOA enables the use of existing quantum computers alongside classical workflows. The team used [Amazon Braket Hybrid Jobs](https://aws.amazon.com/blogs/aws/introducing-amazon-braket-hybrid-jobs-set-up-monitor-and-efficiently-run-hybrid-quantum-classical-workloads/), which allow the full QAOA workflow to be submitted as a single job.

### Benefits of Braket Hybrid Jobs

* **Reduced queue time**: The algorithm only waits in the Braket queue once, rather than waiting for each individual quantum circuit submission
* **Cached circuit compilation**: Subsequent circuits with similar structure reuse previously compiled circuits, saving time and cost

### Integration with Strangeworks Platform

Users can access Braket and these features via the [Strangeworks Compute platform](https://strangeworks.com/). The platform provides an online job management system and downloadable SDK, allowing access to StrangeworksQAOA, Braket Hybrid Jobs, quantum and quantum-inspired devices, and classical solvers.

## Benchmark Methodology

To benchmark StrangeworksQAOA, the team evaluated the aircraft loading optimization problem, initially formulated as part of the Airbus quantum computing challenge. The problem aims to maximize the total load on the aircraft while ensuring that the number of containers (n) fits within the available spaces (N).

### Test Cases

**Table 1: Values for number of containers (n) and available spaces (N)**

| Containers (n) | Spaces (N) | Qubits/Variables |
| -------------- | ---------- | ---------------- |
| 5              | 3          | 26               |
| 6              | 4          | 38               |
| 6              | 5          | 46               |
| 7              | 5          | 52               |
| 7              | 6          | 61               |
| 8              | 6          | 68               |
| 8              | 7          | 78               |

### Comparison Results

![Figure 1: QAOA algorithm comparison](/images/Blog/Blog_3/bl3_1.png)

*Figure 1: Comparison of the StrangeworksQAOA algorithm (solid circles) running on Rigetti Ankaa-3 QPU with Standard QAOA (solid squares), both using Braket Hybrid Jobs. Also shown are Rigetti QRR (hatched squares) and Strangeworks QRR (hatched circles).*

{{% notice tip %}}
Results are averaged over 8 runs. Error bars are small relative to the plot width, indicating stable performance for the considered problem instances.
{{% /notice %}}

### Gradient Analysis

**Table 2: Gradient of the best-fit line for each algorithm**

| Algorithm             | Gradient |
| --------------------- | -------- |
| Gurobi                | -1.25    |
| StrangeworksQAOA      | 3.47     |
| Standard QAOA         | n.a      |
| Strangeworks QRR QAOA | -0.16    |
| Rigetti QRR QAOA      | 1.06     |

{{% notice warning %}}
Standard QAOA grows exponentially, so a linear fit is not reasonable for that case.
{{% /notice %}}

Gradient analysis shows that the Strangeworks variant of QRR is closest to the exact result (Gurobi), indicating it maintains the smallest error as system size increases.

## Details of StrangeworksQAOA

QAOA is an iterative hybrid algorithm cycling between quantum computation and classical optimization.

![Figure 2: QAOA workflow](/images/Blog/Blog_3/bl3_2.png)

*Figure 2: QAOA workflow - (a) parameterized quantum circuit setup; (b) measurement into bitstrings; (c) repeated iterations to build probability distribution; (d) cost function evaluation and classical optimization to generate new parameterized circuits.*

### Workflow Steps

1. **Quantum circuit parameterization**: Start with a set of classical parameters θ. The team used **Real Amplitude Quantum Circuits** instead of the standard QAOA ansatz due to circuit depth limitations.
2. **Run on QPU**: Circuits run on the Rigetti Ankaa-3 QPU, measuring qubits (0 or 1) multiple times to build a probability distribution.
3. **Cost function evaluation**: The probability distribution is fed into a classical cost function.
4. **Optimization**: Classical parameters are updated using **COBYLA** from SciPy.

### Differences in StrangeworksQAOA

In **Standard QAOA**, the solution is usually chosen from the bitstring with the highest probability. For large systems, the probability distribution flattens and each state is only measured once.

In **StrangeworksQAOA**, the team:

* Tracks the lowest cost (C_q) over iterations
* Reports the basis state with minimal cost as the solution
* This is reasonable since the aircraft loading problem has a single classical solution

### Cost Function Formulas

Cost for basis state (q):

![Cost formula](/images/Blog/Blog_3/bl3_3.png)

Average total cost:

![Total cost formula](/images/Blog/Blog_3/bl3_4.png)

Where:

* (q_i \in {0,1}) is the i-th bit of basis state q
* (J_{i,j}) are the QUBO/graph coupling coefficients
* (N_q) is the number of times the basis state was measured

## Conclusions and Outlook

Analysis demonstrates the value of optimizing quantum algorithms for specific applications. Key findings:

* **StrangeworksQAOA** is more robust against hybrid quantum-classical algorithm limitations
* The combination of the Strangeworks platform, StrangeworksQAOA, Braket Hybrid Jobs, and Rigetti Ankaa-3 hardware provides a practical path to solving optimization problems
* Careful tuning achieves superior results without additional cost

{{% notice success %}}
Reliability and reproducibility on the aircraft loading problem suggest a promising path toward achieving **practical quantum advantage**.
{{% /notice %}}

### Getting Started

To explore these algorithms for your own optimization challenge:

* Visit the [Strangeworks platform](https://strangeworks.com/)
* See the [QAOA documentation](https://docs.strangeworks.com/algorithms/qaoa)
* Start with Amazon Braket

## References

1. Romero, S., Osaba, E., Villar-Rodriguez, E., Oregi, I. & Ban, Y. *Hybrid approach for solving real-world bin packing problem instances using quantum annealers.* Sci Rep 13, 11777 (2023).
2. Farhi, E., Goldstone, J. & Gutmann, S. *A Quantum Approximate Optimization Algorithm.* arXiv:1411.4028 (2014).
3. Dupont, M. & Sundar, B. *Extending relax-and-round combinatorial optimization solvers with quantum correlations.* PhysRevA.109.012429 (2024).
4. [Airbus Quantum Computing Challenge](https://www.airbus.com/sites/g/files/jlcbta136/files/2021-10/Airbus-QuantumComputing-Challenge-PS5.pdf)
5. Pilon, G., Gugole, N. & Massarenti, N. *Aircraft Loading Optimization -- QUBO models under multiple constraints.* arXiv:2102.09621 (2021).
6. Guerreschi, G. G. *Solving Quadratic Unconstrained Binary Optimization with divide-and-conquer and quantum algorithms.* arXiv:2101.07813v1 (2021).
7. [Gurobi Optimization](https://www.gurobi.com/)
8. [QAOA Circuit Ansatz](https://qiskit.org/documentation/stubs/qiskit.circuit.library.RealAmplitudes.html)
9. [Real Amplitude Quantum Circuit Ansatz](https://qiskit.org/documentation/stubs/qiskit.circuit.library.RealAmplitudes.html)
10. [Braket QAOA Example](https://github.com/amazon-braket/amazon-braket-examples/blob/main/examples/pennylane/2_Graph_optimization_with_QAOA/2_Graph_optimization_with_QAOA.ipynb)

## About the Authors

### Stuart Flannigan

Senior Quantum Software Engineer at Strangeworks, with experience analyzing quantum simulation experiments. Since joining Strangeworks three years ago, he has worked on integrating theoretical ideas from quantum systems into classical workflows and AI, making the technology more accessible to industry.

### Andrew J. Ochoa

Chief Scientist at Strangeworks, leading R&D initiatives. Holds a Ph.D. in Physics from Texas A&M University and an MBA from UT McCombs. He has multiple publications in quantum computing and has worked at Strangeworks since 2018.

### Michael Brett

![Michael Brett](/images/Blog/Blog_3/blavt1.png)
Principal Specialist for Quantum Computing in the High Performance Computing group at AWS. Leads global business development and marketing efforts for Amazon Braket. Formerly SVP for Applications at Rigetti Computing and CEO of QxBranch.

### Charunethran Panchalam Govindarajan

![Charunethran Panchalam Govindarajan](/images/Blog/Blog_3/blavt2.png)
Sr. Product Marketing Manager at AWS, focusing on High-Performance Computing and Quantum Technologies. Holds a Master’s in Electrical Engineering from Stanford University. Outside work, enjoys drawing and philosophical discussions.

---

*Published on September 2, 2025 in [Amazon Braket](https://aws.amazon.com/blogs/quantum-computing/category/quantum-technologies/amazon-braket/), [Quantum Technologies](https://aws.amazon.com/blogs/quantum-computing/category/quantum-technologies/)*
