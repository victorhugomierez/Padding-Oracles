# Introduction

Encryption is essential for maintaining data security, but even robust encryption can fail if it is not implemented correctly. One example is the ‘Padding Oracle’ attack, a vulnerability that exploits the way encrypted data is processed, particularly when padding is used.

‘Padding Oracle’ attacks occur when an application reveals whether the padding of the encrypted data is correct or not through detailed error messages or variations in response time. Attackers can exploit these subtle clues to deduce the original data without the encryption key. This attack targets encryption methods such as Cipher Block Chaining (CBC), which uses padding to handle data of varying lengths. The ‘Padding Oracle’ attack is so named because the server acts as an ‘oracle’ by providing information on whether the padding in the ciphertext is valid.
Learning Objectives
Throughout this session, you will gain a comprehensive understanding of the following key concepts:

    Padding schemes
    Block cipher modes
    Encryption and decryption mechanisms
    How does the padding oracle attack work?
    Automation mechanisms
    Mitigation and best practices

## Block Ciphers and PKCS#7 Padding

- Block Ciphers: Algorithms such as AES operate on fixed‑size blocks (e.g. 16 bytes). If the plaintext is longer, it is split into blocks; if shorter, padding is added.

- Chaining Modes: Since encryption involves multiple blocks, methods like Cipher Block Chaining (CBC) are used to link them securely.

- Padding: Ensures plaintext fits the block size. Without proper padding, decryption errors or vulnerabilities can occur.

Schemes: Common padding schemes include PKCS#7, ANSI X.923, and ISO/IEC 7816‑4.

PKCS#7:

- Adds bytes where each padding byte’s value equals the number of padding bytes added.

- If the plaintext already matches the block size, an extra block of padding is appended, filled with the block size value.

Example (block size = 8):

- Plaintext length mod 8 = 5 → 3 padding bytes added.

- Each padding byte has value 0x03.

- Final block ends with ...030303.

