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

## PKCS#7 Padding (Block Size = 8)
Concept
Block ciphers such as AES require each block to be of a fixed size (in this case, 8 bytes).

If the plaintext is shorter than this size, padding bytes are added.

Each padding byte has a value equal to the number of bytes added.

If the text is already a multiple of the block size, a full block of padding is added.

Reference table:

![Cryptography Basics](assets/img01.png)


### Practical examples
- Message: ‘victor’

Length = 6 bytes → 6 mod 8 = 6

2 bytes of padding are added → 0x02 0x02

Result: victor\x02\x02

- Message: ‘hugo’

Length = 4 bytes → 4 mod 8 = 4

4 padding bytes are added → 0x04 0x04 0x04 0x04

Result: hugo\x04\x04\x04\x04

- Message: ‘mierez’

Length = 6 bytes → 6 mod 8 = 6

2 bytes of padding are added → 0x02 0x02

Result: mierez\x02\x02

- Each message is independently adjusted to the 8-byte block size, ensuring it can be correctly encrypted using AES or another block cipher.

![Cryptography Basics](assets/img02.png)
