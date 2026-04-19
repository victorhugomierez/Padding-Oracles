
# Padding Oracles – Introduction

![Status](https://img.shields.io/badge/status-active-brightgreen)
![Language](https://img.shields.io/badge/language-Python-blue)
![Focus](https://img.shields.io/badge/focus-Cryptography-orange)

## 📖 Overview
This repository provides a structured introduction to **Padding Oracle Attacks**, a class of cryptographic vulnerabilities that exploit improper error handling in block cipher modes such as AES-CBC. The content is designed for security learners, penetration testers, and developers who want to understand both the exploitation process and the defensive measures.

The project demonstrates:
- The **mathematical foundation** of CBC decryption.  
- How an **oracle** leaks information about padding validity.  
- The **step-by-step recovery** of plaintext without knowing the secret key.  
- Practical examples, pseudocode, and scripts to illustrate the attack.  

---

## 🔑 Key Concepts
- **CBC Decryption Formula**:  
  

\[
  P_i = D_k(C_i) \oplus C_{i-1}
  \]

  
- **Oracle Behaviour**: Systems that reveal “valid” vs “invalid” padding responses.  
- **Byte-by-Byte Recovery**: Manipulating ciphertext to deduce intermediate values.  
- **Plaintext Extraction**: Reconstructing the original message (e.g., `"victorhugo"`) after removing PKCS#7 padding.  

---

## 🛠️ Tools & Automation
- **Manual Scripts**: Python examples using `pycryptodome` for local oracle simulation.  
- **PadBuster**: A Perl-based tool that automates padding oracle exploitation against live endpoints.  

---

## 🎯 Audience
- **Pentesters**: Learn how to identify and exploit padding oracle vulnerabilities during assessments.  
- **Students & Researchers**: Gain hands-on understanding of cryptographic weaknesses.  
- **Secure Coders**: Recognise insecure error handling patterns and apply mitigations.  

---

## 🔒 Mitigation Measures
- Use **authenticated encryption** (AES-GCM, AES-CCM).  
- Avoid revealing detailed error messages in production.  
- Validate inputs securely before decryption.  
- Keep cryptographic libraries updated.  

---

## 📂 Repository Structure
- `Introduction.md` → Theoretical foundation and worked example.  
- `scripts/` → Python scripts for local oracle simulation.  
- `examples/` → PadBuster usage scenarios.  
- `docs/` → Extended notes and diagrams.  

---

## 🚀 Getting Started
Clone the repository and explore the introduction:

```bash
git clone https://github.com/victorhugomierez/Padding-Oracles.git
cd Padding-Oracles


python3 scripts/oracle_attack.py




---

Would you like me to also prepare a **bilingual version (English/Spanish)** of this README so your portfolio reflects your bilingual documentation preference and appeals to international recruiters?
