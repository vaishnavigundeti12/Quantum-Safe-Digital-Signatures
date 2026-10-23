# 🔐 Layered Quantum-Safe Digital Signature Framework

### A Hybrid Cryptographic Architecture for Secure Financial Transactions

**Combining RSA-2048, CRYSTALS-Dilithium (ML-DSA-65), and Falcon-512**

## 📌 Overview

The emergence of quantum computing presents a significant threat to traditional public-key cryptographic algorithms such as RSA. Financial systems, digital banking platforms, and payment networks require secure and reliable digital signatures to maintain transaction authenticity and integrity.

This project presents a **layered quantum-safe digital signature framework** that combines classical cryptography with post-quantum cryptographic algorithms to support a controlled transition toward quantum-resistant security.

The framework implements a strict cryptographic dependency chain in which each signature layer authenticates the preceding layer. This design aims to detect signature tampering, unauthorized layer removal, and downgrade attempts while retaining compatibility with existing cryptographic infrastructure.

A Python-based prototype uses the Open Quantum Safe `liboqs` library to perform post-quantum cryptographic operations.

## 🎯 Objectives

- Develop a hybrid digital signature framework combining classical and post-quantum cryptography.
- Strengthen financial transaction authentication against future quantum threats.
- Detect tampering and downgrade attempts through chained signature verification.
- Integrate certificate validation and trusted timestamp mechanisms.
- Evaluate signature sizes, signing and verification latency, and cryptographic overhead.
- Support crypto-agility to facilitate future cryptographic algorithm migration.

## 🏗️ System Architecture

The framework uses a layered signing and verification process.

### Digital Signature Generation

```text
Transaction Data
       |
       v
   SHA-256 Hash
       |
       v
   RSA-2048 Sign
       |
       v
 ML-DSA-65 Sign
 (CRYSTALS-Dilithium)
       |
       v
   Falcon-512 Sign
       |
       v
 Secure Transaction Packet
       |
       v
      Receiver
```

### Verification Process

1. Validate the sender's X.509 certificate.
2. Verify the RSA-2048 signature.
3. Verify the ML-DSA-65 signature.
4. Verify the Falcon-512 signature.
5. Validate the trusted timestamp and transaction integrity.
6. Check for missing signature layers or downgrade attempts.
7. Accept the transaction only when the required verification checks succeed.

The prototype also incorporates a Certificate Authority (CA), a simulated Hardware Security Module (HSM), a Trusted Timestamp Authority (TSA), a secure packet registry, and a crypto-agile configuration interface.

## 🔑 Cryptographic Algorithms

| Algorithm | Purpose | Standard / Reference |
|---|---|---|
| RSA-2048 | Classical digital signature and legacy compatibility | PKCS #1 / RSA-PSS |
| ML-DSA-65 | Post-quantum digital signature | NIST FIPS 204 |
| Falcon-512 | Additional lattice-based signature layer | Falcon algorithm |
| SHA-256 | Transaction hashing and integrity | NIST FIPS 180-4 |
| RSA-4096 | Root certificate signing in the prototype | X.509 PKI |
| RSA-PSS | Signature and timestamp-related operations | RSA signature scheme |

## ✨ Key Features

### 1. Layered Hybrid Cryptography
Combines a conventional RSA signature with two post-quantum signature layers to provide defense in depth.

### 2. Downgrade Attack Detection
The signature dependency chain is designed to reject packets when required signature layers are removed or invalidated.

### 3. Quantum-Threat Mitigation
The framework retains lattice-based signature layers to reduce dependence on RSA for long-term security against potential quantum attacks.

### 4. Certificate-Based Identity Validation
Uses X.509 certificates, a Certificate Authority, and certificate revocation checks to validate enrolled identities.

### 5. Trusted Timestamping
Integrates a Trusted Timestamp Authority with signed timestamps and nonces to support transaction auditing and replay-attack mitigation.

### 6. Performance Optimization
Explores server-side pre-computation to reduce transaction-time signing overhead in point-of-sale and mobile payment environments.

### 7. Crypto-Agility
Provides a configurable interface for replacing supported post-quantum algorithms as cryptographic requirements evolve.

## 🛠️ Technology Stack

- **Programming Language:** Python 3.12
- **Development Environment:** Jupyter Notebook
- **Cryptographic Library:** Open Quantum Safe `liboqs` v0.15.0
- **Python Binding:** `liboqs-python`
- **Cryptographic Operations:** RSA, RSA-PSS, SHA-256, ML-DSA-65, Falcon-512
- **Security Infrastructure:** X.509 certificates, CA, TSA, simulated HSM
- **Testing and Analysis:** Signature verification, tamper detection, downgrade testing, performance measurements

## ⚙️ Implementation Workflow

The prototype follows these stages:

1. **Identity Enrollment:** Register users and issue X.509 certificates.
2. **Key Generation:** Generate the required RSA and post-quantum key pairs.
3. **Transaction Hashing:** Generate a SHA-256 hash of the transaction data.
4. **Layered Signing:** Generate the RSA signature, sign it using ML-DSA-65, and then sign the ML-DSA-65 signature using Falcon-512.
5. **Secure Packet Creation:** Package the transaction data and its associated signatures with the required verification metadata.
6. **Packet Transmission:** Transfer the packet to the receiving system.
7. **Layered Verification:** Validate the certificate, signatures, timestamp, and integrity checks.
8. **Security Decision:** Accept valid transactions and reject packets that fail mandatory checks.

## 📊 Evaluation and Results

The research evaluates the prototype using actual post-quantum cryptographic operations provided by `liboqs`, rather than relying exclusively on simulated post-quantum parameters.

The evaluation covers:

- Public and private key sizes.
- Digital signature sizes.
- Signing and verification latency.
- Transaction integrity validation.
- Modified-document rejection.
- Missing-layer and downgrade detection.
- Certificate revocation handling.
- Server-side pre-computation.

### Reported Signature Sizes

| Algorithm | Signature Size |
|---|---:|
| RSA-2048 | 256 bytes |
| ML-DSA-65 | 3,309 bytes |
| Falcon-512 | Approximately 657 bytes |

The Falcon-512 signature size shown is the measured average reported in the paper; Falcon signatures can vary in length.

The combined signature payload is approximately **4,222 bytes**, excluding additional packet metadata, certificates, and timestamps.

The paper reports an approximate total signing latency of 1.1–2.7 ms and total verification latency of 0.2–0.5 ms under its prototype test conditions. These are implementation-specific measurements and should not be interpreted as universal performance guarantees.

## 🧪 Security Testing

The prototype's security evaluation focuses on the following scenarios:

- **Valid transaction:** All required verification checks succeed.
- **Transaction modification:** Changes to signed transaction data are detected.
- **Invalid signature:** A failed signature verification causes rejection.
- **Downgrade attempt:** Removal of required post-quantum signature layers triggers verification failure or downgrade detection.
- **Certificate revocation:** Revoked identities are rejected during certificate validation.
- **Timestamp validation:** Invalid trusted timestamp information causes rejection.

## 🌍 Potential Applications

- Digital banking and payment gateways.
- UPI-like payment infrastructure.
- Point-of-sale terminals and mobile banking.
- Digital identity and public-key infrastructure.
- Secure financial messaging.
- Transaction audit trails and timestamped digital records.

## 🚀 Future Scope

- Integration with physical, PKCS #11-compatible Hardware Security Modules.
- Formal cryptographic security proofs for the chained signature construction.
- Benchmarking across different hardware platforms and operating environments.
- Evaluation of packet-size and certificate-storage overhead at scale.
- Testing interoperability with existing banking and PKI infrastructure.
- Migration toward fully post-quantum authentication as appropriate standards and deployment requirements evolve.

## 📁 Repository Structure

A suggested organization for the repository is:

```text
layered-quantum-safe-signatures/
│
├── README.md
├── notebooks/
│   └── quantum_safe_signature_demo.ipynb
├── src/
│   ├── key_management.py
│   ├── signature_generation.py
│   ├── signature_verification.py
│   ├── certificate_authority.py
│   ├── timestamp_authority.py
│   └── crypto_agility.py
├── tests/
│   ├── test_signatures.py
│   ├── test_tampering.py
│   └── test_downgrade.py
├── results/
│   └── benchmark_results.csv
├── docs/
│   └── architecture.md
└── requirements.txt
```

*Note: This is a suggested repository structure. Adjust the filenames to match the files actually implemented in your project.*

## ▶️ Getting Started

### Prerequisites

- Python 3.12
- Jupyter Notebook
- A compatible installation of the Open Quantum Safe `liboqs` C library
- The corresponding `liboqs-python` binding

### Setup

1. Clone the repository:

   ```bash
   git clone https://github.com/YOUR_USERNAME/YOUR_REPOSITORY.git
   cd YOUR_REPOSITORY
   ```

2. Create and activate a virtual environment:

   ```bash
   python -m venv .venv
   ```

   Windows:

   ```bash
   .venv\Scripts\activate
   ```

   Linux/macOS:

   ```bash
   source .venv/bin/activate
   ```

3. Install the Python dependencies after configuring the compatible `liboqs` installation:

   ```bash
   pip install jupyter liboqs-python cryptography
   ```

4. Launch Jupyter Notebook:

   ```bash
   jupyter notebook
   ```

5. Open the project notebook and execute the cells in order.

**Important:** The native `liboqs` library and its Python binding must be installed in a compatible configuration. The commands above provide a starting point; additional installation steps may be necessary depending on your operating system and the binding version.

## 👥 Authors

**Vaishnavi Gundeti · Kanishka Pandya · Rishi Bhanushali · Aripta Khandare · Shravan Rangdale**

Department of Electronics Engineering  
VLSI Design & Technology  
Shah and Anchor Kutchhi Engineering College, Mumbai, India

## 📚 References

- Open Quantum Safe — [`liboqs`](https://github.com/open-quantum-safe/liboqs)
- NIST — [FIPS 204: Module-Lattice-Based Digital Signature Standard](https://csrc.nist.gov/pubs/fips/204/final)
- NIST — [Post-Quantum Cryptography](https://csrc.nist.gov/projects/post-quantum-cryptography)
- RSA cryptographic standards — [RFC 8017](https://www.rfc-editor.org/rfc/rfc8017)

For the complete research background, methodology, architecture, and evaluation, refer to the accompanying research paper.

## ⚠️ Disclaimer

This repository describes an academic research prototype. It is not a production-ready banking security system and has not been established as a formally verified cryptographic construction. The simulated HSM, certificate infrastructure, timing measurements, and security claims should be assessed within the prototype's documented scope before any real-world deployment.

---

**Keywords:** Post-Quantum Cryptography · Digital Signatures · RSA · ML-DSA · Dilithium · Falcon · Quantum-Safe Security · Financial Security · Cryptographic Agility · Downgrade Attack Detection
