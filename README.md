# Adoption of Post-Quantum Cryptography: An Empirical Performance Study

## Overview

This project provides an empirical evaluation of modern Post-Quantum Cryptography (PQC) algorithms using the Open Quantum Safe (liboqs) framework. The study benchmarks NIST-standardized post-quantum algorithms and compares their computational performance, key sizes, and communication overhead against traditional public-key cryptographic systems.

The notebook generates real-world performance measurements using actual hardware execution rather than theoretical estimates, enabling researchers, students, and cybersecurity professionals to better understand the practical implications of PQC adoption.

---

## Research Objectives

This study aims to:

* Evaluate the performance of NIST-standardized PQC algorithms.
* Compare post-quantum cryptography with classical cryptographic systems.
* Measure key generation, encapsulation, decapsulation, signing, and verification times.
* Analyze communication overhead introduced by larger key and ciphertext sizes.
* Provide reproducible empirical evidence for PQC deployment decisions.

---

## Algorithms Evaluated

### Classical Cryptography

* RSA-2048
* ECDSA P-256

### Post-Quantum Cryptography

#### Key Encapsulation Mechanisms (KEM)

* ML-KEM-768 (formerly CRYSTALS-Kyber)

#### Digital Signatures

* ML-DSA-65 (formerly CRYSTALS-Dilithium)

All PQC implementations are executed using the Open Quantum Safe (liboqs) framework.

---

## Features

* Automated installation of Open Quantum Safe dependencies
* Real hardware benchmarking
* Statistical analysis using multiple iterations
* Performance metrics collection
* CSV-based result storage
* Communication overhead analysis
* Aggregate summary generation
* Google Drive integration for persistent storage
* Publication-ready datasets

---

## Experimental Methodology

For each algorithm, the notebook performs repeated execution trials to obtain statistically meaningful measurements.

Metrics collected include:

### Key Encapsulation Mechanisms

* Key Generation Time
* Encapsulation Time
* Decapsulation Time
* Public Key Size
* Ciphertext Size

### Digital Signatures

* Key Generation Time
* Signing Time
* Verification Time
* Public Key Size
* Signature Size

### Statistical Outputs

* Mean
* Standard Deviation
* Minimum
* Maximum
* Confidence Intervals

---

## Project Structure

```text
Adoption_of_Post_Quantum_Cryptography.ipynb
│
├── Benchmark Execution
│   ├── Install liboqs
│   ├── Execute PQC algorithms
│   └── Capture timing data
│
├── Statistical Analysis
│   ├── Aggregate results
│   ├── Compute descriptive statistics
│   └── Export datasets
│
├── Communication Overhead Analysis
│   ├── Key size comparison
│   ├── Ciphertext comparison
│   └── Network impact evaluation
│
└── Final Summary Generation
```

---

## Output Files

The notebook generates CSV datasets that can be used for:

* Academic research
* Statistical analysis
* Visualization
* Publication figures
* Reproducibility studies

Example outputs:

```text
extended_stats.csv
summary_statistics.csv
communication_overhead.csv
```

---

## Requirements

### Python Packages

```bash
pip install pandas numpy matplotlib scipy oqs
```

### Environment

* Python 3.10+
* Google Colab (recommended)
* Google Drive account (optional for storage)
* Open Quantum Safe (liboqs)

---

## Running the Study

### Clone Repository

```bash
git clone https://github.com/yourusername/adoption-of-post-quantum-cryptography.git

cd adoption-of-post-quantum-cryptography
```

### Launch Notebook

```bash
jupyter notebook
```

or open directly in Google Colab.

### Execute Cells

Run all cells sequentially:

1. Install dependencies
2. Mount Google Drive
3. Execute benchmarking experiments
4. Generate statistics
5. Export results

---

## Example Research Questions

* How does ML-KEM-768 compare to RSA-2048 in computational performance?
* What communication overhead is introduced by PQC deployment?
* Can current hardware efficiently support NIST PQC standards?
* What trade-offs exist between security and performance?
* How significant are the larger key sizes in real-world deployments?

---

## Applications

This project supports research and education in:

* Cybersecurity
* Cryptography
* Post-Quantum Security
* Secure Communications
* Network Security
* Government and Defense Systems
* Critical Infrastructure Protection
* Academic Instruction

---

## Educational Use

The notebook is suitable for:

* Undergraduate cybersecurity courses
* Graduate cryptography courses
* Research methodology instruction
* Quantum-safe security workshops
* Capstone and thesis projects

---

## Future Enhancements

* Additional NIST PQC candidates
* ARM vs x86 benchmarking
* Cloud infrastructure testing
* Energy consumption analysis
* Hybrid classical/post-quantum deployments
* Automated visualization dashboards


