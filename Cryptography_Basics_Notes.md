# Cryptography Basics Notes

## What is Cryptography?

Cryptography is the science of protecting information by converting it into a form that unauthorized users cannot understand.

### Main Objectives
- **Confidentiality** – Only authorized users should access the data.
- **Integrity** – Ensure data is not modified.
- **Authentication** – Verify identity.

---

## Key Terms

| Term | Meaning |
|-------|---------|
| Plaintext | Original readable message/data |
| Ciphertext | Encrypted/scrambled message |
| Encryption | Converting plaintext into ciphertext |
| Decryption | Converting ciphertext back into plaintext |
| Cipher | Algorithm used for encryption/decryption |
| Key | Secret value used by the cipher |

---

## Real-World Uses of Cryptography

### Secure Website Login
- User credentials are encrypted before being sent to the server.

### SSH Connections
```bash
ssh user@server
```
- Creates an encrypted tunnel between client and server.

### Online Banking
- Uses HTTPS and digital certificates.
- Encrypts communication and verifies server identity.

### File Verification
```bash
sha256sum file.iso
```
- Hash functions verify file integrity.

---

## Compliance Standards

### Payment Card Information
Organizations handling card data must follow **PCI DSS**.

Data must be encrypted:

1. **At Rest** – While stored in databases/disks.
2. **In Motion** – While transmitted over networks.

### Medical Record Protection

| Regulation | Region |
|------------|--------|
| HIPAA | USA |
| HITECH | USA |
| GDPR | European Union |
| DPA | United Kingdom |

---

## Caesar Cipher

One of the oldest and simplest encryption techniques.

### Example

```text
Plaintext : TRYHACKME
Key       : 3
Ciphertext: WUBKDFNPH
```

### Characteristics
- Each letter is shifted by a fixed number of positions.
- Vulnerable to brute-force attacks.
- Only 25 valid keys.

---

## Symmetric Encryption

Uses the **same key** for encryption and decryption.

### Features
- Fast and efficient.
- Uses a shared secret key.

### Challenges
- Key distribution problem.
- Key leakage risk.

### Popular Algorithms

#### DES (Data Encryption Standard)
- Adopted: 1977
- Key Size: 56-bit
- Insecure by modern standards.

#### 3DES
- DES applied three times.
- Key Size: 168-bit
- Effective Security: 112-bit
- Deprecated in 2019.

#### AES (Advanced Encryption Standard)
- Adopted: 2001
- Key Sizes:
  - 128-bit
  - 192-bit
  - 256-bit

### Uses
- HTTPS
- Wi-Fi
- VPN
- Disk Encryption
- Messaging Apps

### Advantages
- Fast
- Efficient for large data
- Less computational power required

### Disadvantages
- Secure key sharing is difficult.
- Difficult to manage for many users.

---

## Asymmetric Encryption

Uses two keys:

1. **Public Key** – Shared with everyone.
2. **Private Key** – Kept secret.

### Popular Algorithms

#### RSA
Recommended key sizes:
- 2048-bit
- 3072-bit
- 4096-bit

#### Diffie-Hellman (DH)
- Secure key exchange protocol.
- Minimum recommended key size: 2048-bit.

#### ECC (Elliptic Curve Cryptography)
- Efficient and secure.
- 256-bit ECC ≈ 3072-bit RSA security.

### Why Asymmetric Encryption is Secure?
Based on mathematical problems that are easy to compute but extremely difficult to reverse.

---

## XOR Operation

XOR (Exclusive OR) is a logical operation used on binary values.

### XOR Truth Table

| A | B | A ⊕ B |
|---|---|-------|
| 0 | 0 | 0 |
| 0 | 1 | 1 |
| 1 | 0 | 1 |
| 1 | 1 | 0 |

### Rule
- Same bits → 0
- Different bits → 1

### Example

```text
1010 ⊕ 1100 = 0110
```

### Properties

#### Property 1
```text
A ⊕ A = 0
```

Example:
```text
1010 ⊕ 1010 = 0000
```

#### Property 2
```text
A ⊕ 0 = A
```

Example:
```text
1011 ⊕ 0000 = 1011
```

#### Property 3 (Commutative)
```text
A ⊕ B = B ⊕ A
```

#### Property 4 (Associative)
```text
(A ⊕ B) ⊕ C = A ⊕ (B ⊕ C)
```

### XOR in Cryptography

Variables:
- P = Plaintext
- K = Secret Key
- C = Ciphertext

Encryption:

```text
C = P ⊕ K
```

Example:

```text
P = 1010
K = 1100

1010 ⊕ 1100 = 0110
```

Ciphertext:

```text
C = 0110
```

Decryption:

```text
C ⊕ K = P
```

Example:

```text
0110 ⊕ 1100 = 1010
```

---

## Modulo Operation

Modulo (`%`) returns the remainder after division.

### Examples

```text
25 % 5 = 0
23 % 6 = 5
23 % 7 = 2
```

### Important Property

For:

```text
a % n
```

The result always lies between:

```text
0 and n - 1
```

### Example

```text
x % 5
```

Possible outputs:

```text
0, 1, 2, 3, 4
```

---

## Quick Revision

### Symmetric Encryption
- One Key
- Fast
- Examples: AES, DES, 3DES

### Asymmetric Encryption
- Two Keys
- Public + Private Key
- Examples: RSA, DH, ECC

### Cryptography Goals
- Confidentiality
- Integrity
- Authentication
