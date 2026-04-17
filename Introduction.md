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


# Modes of Operation in Block Ciphers
Block ciphers such as AES encrypt data in fixed‑size chunks (blocks). Since plaintext rarely aligns perfectly with block boundaries, and encrypting blocks independently can expose patterns, modes of operation were developed. These modes define how blocks are processed and linked to achieve confidentiality and integrity.

--- 

1. Electronic Codebook (ECB)
- How it works: Each plaintext block is encrypted independently.

- Weakness: Identical plaintext blocks produce identical ciphertext blocks, revealing patterns.

-  Example:

    - Plaintext: ATTACK ATTACK

    - Ciphertext (ECB): [C1][C1] → repetition visible.

- Use case: Rarely recommended, sometimes used for small, random data (e.g. keys).

2. Cipher Block Chaining (CBC)
- How it works: Each plaintext block is XORed with the previous ciphertext block before encryption. The first block uses an Initialisation Vector (IV).

- Strength: Identical plaintext blocks encrypt differently if their positions or IVs differ.

- Example:

    - Plaintext: ATTACK ATTACK

    - Ciphertext (CBC): [C1][C2] → no repetition visible.

- Use case: Secure file encryption, provided IVs are random and padding is handled correctly.

3. Counter (CTR) Mode
- How it works: A counter value combined with a nonce is encrypted, and the result is XORed with the plaintext. Each block uses a unique counter, so blocks are independent but secure.

- Strength: Fast, parallelisable, and avoids the need for padding.

- Example:

    - Counter values: CTR1, CTR2

    - Ciphertext: [C1][C2] → each block unique due to counter.

- Use case: High‑performance applications such as network encryption (TLS, VPNs).

### Key Takeaway
- ECB: Simple but insecure for patterns.

- CBC: Secure chaining with IVs, widely used.

- CTR: Efficient, parallelisable, and padding‑free.


## Cipher Block Chaining (CBC) Mode
CBC is a widely used encryption mode that secures plaintext by combining each block with the previous ciphertext block before encryption. This chaining introduces randomness and ensures that identical plaintext blocks produce different ciphertexts depending on their position and the Initialisation Vector (IV).

### How Encryption Works in CBC Mode
Plaintext: "victorhugo"  
Block size: 8 bytes → split into two blocks:

Block 1: "victorhu"

Block 2: "go" + padding (0x06 × 6) → "go\x06\x06\x06\x06\x06\x06"

- Step 1: Initialisation Vector (IV)
A random IV of 8 bytes is generated.

Block 1 is XORed with the IV before encryption.

This ensures the first ciphertext block is unique even if the plaintext repeats.

- Step 2: Encrypt Block 1
Result of Plaintext Block 1 ⊕ IV is encrypted with AES.

Output = Ciphertext Block 1 (C1).

- Step 3: Chain Block 2
Block 2 ("go\x06...") is XORed with Ciphertext Block 1 (C1).

This links the second block to the first, preventing repetition patterns.

- Step 4: Encrypt Block 2
Result of Plaintext Block 2 ⊕ C1 is encrypted with AES.

Output = Ciphertext Block 2 (C2).

```
Plaintext:  victorhu | go\x06\x06\x06\x06\x06\x06
IV:         random8
Step 1:     victorhu ⊕ IV → Encrypt → C1
Step 2:     (go+padding) ⊕ C1 → Encrypt → C2
Ciphertext: C1 | C2
```
### Key Points
IV ensures randomness in the first block.

Chaining prevents patterns across identical plaintext blocks.

Padding ensures each block fits the required size.

CBC is secure if IVs are random and never reused.


## CBC Mode Example – "victorhugo"
- Step 1: Initialisation Vector (IV) and Splitting
Plaintext length = 10 characters.

Block size = 8 bytes → split into two blocks:

- Block 1: "victorhu" (8 bytes, no padding).

- Block 2: "go" (2 bytes) + 6 bytes of padding (\x06) → "go\x06\x06\x06\x06\x06\x06".

IV chosen: 01 01 01 01 01 01 01 01.

## Step 2: XOR Block 1 with IV
Block 1 hex: 76 69 63 74 6F 72 68 75.

- XOR with IV:

```
76 69 63 74 6F 72 68 75
```

⊕ 01 01 01 01 01 01 01 01
= 77 68 62 75 6E 73 69 74

```
Código
- This intermediate value introduces randomness.

---

### Step 3: Encrypt the XOR Result
- Encrypt `77 68 62 75 6E 73 69 74` with AES/DES and secret key.  
- Output = Ciphertext Block 1 (`C1`): e.g. `A3 3C 9F 12 58 44 76 10`.  
- `C1` now acts as the IV for the next block.

---

### Step 4: XOR and Encrypt Block 2
- Block 2 hex: `67 6F 06 06 06 06 06 06`.  
- XOR with `C1`:  
```

67 6F 06 06 06 06 06 06
⊕ A3 3C 9F 12 58 44 76 10
= C4 53 99 14 5E 42 70 16

```
Código
- Encrypt result → Ciphertext Block 2 (`C2`): e.g. `B7 9F 2D 5A 11 66 4F 7A`.

---

###  Final Ciphertext
The encrypted message is the concatenation of both blocks: 
```

C1 | C2
= A3 3C 9F 12 58 44 76 10 | B7 9F 2D 5A 11 66 4F 7A

```

---

###  Key Insight
- The **IV** ensures randomness in the first block.  
- Each ciphertext block depends on both its plaintext and the previous ciphertext.  
- Padding ensures the final block fits the required size.  
- CBC prevents visible patterns, even if the same plaintext is encrypted multiple times.

---

¿Quieres que te prepare un **diagrama visual** mostrando el flujo (IV → Block 1 → C1 → Block 2 → C2) para que lo uses en tu documentación de laboratorio?
```
![The first block `"victorhu"` is XORed with the Initialisation Vector (IV) before encryption.- The resulting ciphertext block (`C1`) becomes the IV for the next block.- The second block `"go\x06\x06\x06\x06\x06\x06"` is XORed with `C1` and then encrypted to produce `C2`.- The final ciphertext is `C1 | C2`](assets/img03.png)



