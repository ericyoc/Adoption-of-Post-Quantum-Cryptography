# The Adoption of Post-Quantum Cryptography Due to the Impact of Quantum Algorithms

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/ericyoc/Adoption-of-Post-Quantum-Cryptography/blob/main/Adoption_of_Post_Quantum_Cryptography.ipynb)

- **Threat:** a calibrated cost model of factoring RSA moduli with the classical general number field sieve (GNFS) and with Shor's algorithm, plus a projection of when quantum hardware becomes large enough to break each key size.
- **Defense:** a same-machine benchmark of the NIST post-quantum standards (FIPS 203, 204, 205) against the classical algorithms they replace, plus the on-the-wire size of each and of a full TLS 1.3 handshake.

---

## Key Findings

Measured on a Google Colab VM (Intel Xeon @ 2.20 GHz, one pinned core, liboqs 0.16.0):

| Finding | Result |
|---|---|
| ML-KEM vs. elliptic-curve key exchange | ML-KEM is **faster**: every operation < 0.03 ms vs. 0.04–0.09 ms for X25519 / ECDH P-256 |
| ML-DSA-65 vs. ECDSA P-256 | Signing ≈ 4× ECDSA (0.124 vs. 0.032 ms) but 6× faster than RSA-2048; verification faster than ECDSA |
| SLH-DSA (hash-based) | The exception: 128s signs in ≈ 0.76 s, 128f in ≈ 36 ms |
| Signature size | ML-DSA-65 = 3,309 B, 47× a DER-encoded ECDSA signature and 13× an RSA-2048 signature |
| Full TLS 1.3 handshake (2-cert chain) | ML-KEM-768 + ML-DSA-65 carries 16,103 B vs. 407 B for X25519 + ECDSA (+15.7 TB/day at 10⁹ handshakes) |
| Quantum advantage over GNFS | 10³ at 512 bits → 10¹⁸ at 2048 bits → 10²⁹ at 4096 bits |
| Projected year RSA-2048 becomes breakable | ≈ 2035 / 2040 / 2045 if physical qubit counts double every 1 / 1.5 / 2 years (sub-million-qubit estimate) |

**Bottom line:** for the lattice-based standards, the cost of migration is bandwidth, not CPU time. The 2030–2035 window should be treated as a deadline to finish by, not a date to start from.

---

## Algorithms Evaluated

| Role | Post-quantum (liboqs) | Classical baseline (pyca/cryptography) |
|---|---|---|
| Key exchange | ML-KEM-512 / 768 / 1024 (FIPS 203); Kyber512 / 768 / 1024 (pre-standard); X25519MLKEM768 hybrid (sizes) | X25519, ECDH P-256 |
| Signatures | ML-DSA-44 / 65 / 87 (FIPS 204); SLH-DSA-SHA2-128s / 128f (FIPS 205) | RSA-2048 (PSS), ECDSA P-256, Ed25519 |

---

## Notebook Structure

`Adoption_of_Post_Quantum_Cryptography.ipynb`:

| Cell | Purpose | Status |
|---|---|---|
| 1 | Original single-run liboqs benchmark and plot | Superseded by cell 5 (one run per operation) |
| 2 | 1,000-iteration KEM lifecycle statistics → `extended_stats.csv` | Superseded by cell 5 |
| 3 | Hard-coded size comparison plot | Superseded by cell 5 (sizes now measured) |
| 4 | Aggregates CSVs into a summary table | Kept for reference |
| **5** | **Revision cell (standalone)**: everything used in the paper | **Use this** |

### What the revision cell (cell 5) does

1. **Records the environment**: CPU model, ISA flags, RAM, liboqs and liboqs-python versions, liboqs build settings, OpenSSL and pyca/cryptography versions → `env.json`.
2. **Benchmarks every operation**: KeyGen, Encaps/Sign and Decaps/Verify are each timed 1,000 times (100 for SLH-DSA and RSA key generation) after 50 discarded warm-up calls, pinned to one core, with garbage collection disabled. Every result is checked for correctness (decapsulated secret matches; signature verifies).
3. **Times the full KEM lifecycle** (KeyGen + Encaps + Decaps as one unit; Algorithm 1 in the paper).
4. **Runs the classical baselines on the same machine.**
5. **Measures on-the-wire sizes** directly from the generated objects, separately for key exchange and for signatures, and composes them into a TLS 1.3 handshake byte model (key shares + 2 public keys + 3 signatures).
6. **Threat model:**
   - GNFS cost is $L_N[1/3, (64/9)^{1/3}]$, scaled to the RSA-250 factoring record (829 bits, ≈ 2,700 core-years).
   - Shor cost and runtime use the Gidney–Ekerå (2021) Toffoli count and measurement depth, scaled from 8 hours at 2048 bits.
   - The breakability timeline assumes the physical qubits needed for RSA-2048 are < 1 M (Gidney 2025) or 20 M (Gidney–Ekerå 2021), scaled linearly with key size. Capacity starts at 10³ qubits in 2025 and doubles every 1, 1.5 or 2 years.
7. **Writes publication outputs**: vector PDF figures at IEEE column width, CSVs, and `tables.tex` (booktabs tables ready to `\input`).

All model assumptions are constants at the top of the cell, so the projection can be re-run as estimates change.

---

## Output Files

Written to `MyDrive/Real_PQC_Empirical_Study/revision_ccwc2027/` in Colab, or to `./revision_ccwc2027/` when run locally:

| File | Contents |
|---|---|
| `env.json` | Hardware, software, and build record |
| `per_operation_stats.csv` | n, median, mean, std, P99 (ms) for every algorithm and operation |
| `kem_lifecycle_stats.csv` | Full-lifecycle KEM latency distribution |
| `kem_sizes.csv`, `signature_sizes.csv` | Measured on-the-wire sizes and ratios to X25519 / ECDSA |
| `handshake_bytes.csv` | TLS 1.3 handshake bytes and added TB/day at 10⁶ and 10⁹ sessions/day |
| `threat_model.csv` | GNFS core-years, Shor Toffoli count and hours, advantage, per key size |
| `qubit_timeline.csv` | Projected year per key size, qubit-requirement estimate, and growth rate |
| `tables.tex` | LaTeX tables for the paper |
| `fig_factoring_time.pdf/.png` | Classical vs. quantum time to factor |
| `fig_quantum_advantage.pdf/.png` | log₁₀ classical/quantum ratio |
| `fig_crqc_timeline.pdf/.png` | Projected year each RSA key size becomes breakable |

---

## Running the Study

### Google Colab (recommended)

1. Open the notebook with the badge above.
2. Run **cell 5** on its own. In a fresh session, the first import compiles liboqs, which takes about 10 minutes. The benchmarks then take 1–2 minutes.
3. Approve the Google Drive mount when prompted.

### Locally (Linux, x86-64)

```bash
git clone https://github.com/ericyoc/Adoption-of-Post-Quantum-Cryptography.git
cd Adoption-of-Post-Quantum-Cryptography
python3 -m venv .venv && source .venv/bin/activate
pip install numpy pandas matplotlib cryptography \
    git+https://github.com/open-quantum-safe/liboqs-python.git
jupyter notebook   # then run cell 5
```

The first `import oqs` downloads and builds liboqs into `~/_oqs`, which needs `cmake`, `ninja` and a C compiler. Without Google Drive, results go to `./revision_ccwc2027/`.

**Note:** the PyPI package named `oqs` is *not* the Open Quantum Safe binding. Install liboqs-python from GitHub as shown above.

### Requirements

- Python 3.10+
- liboqs-python (GitHub), which builds liboqs ≥ 0.14 (needed for the `SLH_DSA_PURE_*` names; older builds fall back to SPHINCS+)
- `cryptography`, `numpy`, `pandas`, `matplotlib`

---

## Reproducibility Notes

- Timings include the per-call overhead of the Python bindings. The PQC and classical calls share it, so their relative ordering is meaningful. Absolute values will differ with native C callers and on other hardware.
- Colab VMs are shared, so expect run-to-run variation of a few percent. Report medians and P99 rather than single runs.
- The liboqs build is a distributable build: AVX2/AVX-512 code paths are selected at run time when the CPU supports them.
- The threat model is first-order. It omits the $o(1)$ term in GNFS, scales the qubit requirement linearly with key size, and assumes a baseline capacity and growth rate. Treat the dates as scenario outputs, not predictions.

---

## Key References

- NIST FIPS 203 (ML-KEM), FIPS 204 (ML-DSA), FIPS 205 (SLH-DSA), 2024.
- C. Gidney and M. Ekerå, "How to factor 2048 bit RSA integers in 8 hours using 20 million noisy qubits," *Quantum*, 5:433, 2021.
- C. Gidney, "How to factor 2048 bit RSA integers with less than a million noisy qubits," arXiv:2505.15917, 2025.
- F. Boudot et al., "Factorization of RSA-250," 2020.
- K. Kwiatkowski et al., "Post-quantum hybrid ECDHE-MLKEM key agreement for TLSv1.3," draft-ietf-tls-ecdhe-mlkem, 2026.
- Open Quantum Safe project, liboqs: https://openquantumsafe.org

