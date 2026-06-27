# Public Key Cryptography Basics Notes

> Comprehensive notes covering Authentication, Confidentiality, RSA, Diffie-Hellman, SSH Keys, Digital Signatures, Certificates, and GPG.

---

# Table of Contents

1. Security Concepts
2. Symmetric vs Asymmetric Encryption
3. Key Exchange
4. RSA Cryptography
5. Diffie-Hellman Key Exchange
6. SSH Authentication
7. Digital Signatures
8. Certificates and HTTPS
9. GPG / PGP
10. Important Commands
11. Quick Revision

---

# 1. Core Security Concepts

## Authentication

Verifies the identity of a user or system.

**Example:**
Logging into a website using a username and password.

Question answered:

> "Who are you?"

---

## Authenticity

Verifies that a message really came from the claimed sender.

**Example:**
Digital signatures.

Question answered:

> "Who sent this message?"

---

## Integrity

Ensures that data has not been modified.

**Example:**
Hashing.

Question answered:

> "Was the data changed?"

---

## Confidentiality

Ensures that only authorized parties can access information.

**Example:**
Encryption.

Question answered:

> "Can someone else read this information?"

---

# 2. Symmetric vs Asymmetric Encryption

## Symmetric Encryption

Uses the same key for encryption and decryption.

Examples:

* AES
* DES

### Advantages

* Fast
* Efficient

### Disadvantages

* Secure key sharing is difficult

---

## Asymmetric Encryption

Uses two keys:

* Public Key
* Private Key

Examples:

* RSA
* ECC

### Public Key

Can be shared with everyone.

### Private Key

Must remain secret.

---

# Why Use Both?

Asymmetric encryption is slow.

Therefore:

1. Asymmetric cryptography securely exchanges a secret key.
2. Symmetric cryptography encrypts actual communication.

Example:

HTTPS uses:

* RSA/ECC → Key exchange and authentication
* AES → Data encryption

---

# 3. Key Exchange Problem

Problem:

> How can two people share a secret key over an insecure network?

Solution:

Use public key cryptography or Diffie-Hellman.

---

# Lock and Box Analogy

| Analogy     | Cryptography  |
| ----------- | ------------- |
| Lock        | Public Key    |
| Lock Key    | Private Key   |
| Secret Code | Symmetric Key |

---

# 4. RSA Cryptography

## Why is RSA Secure?

RSA security relies on:

> Factoring large numbers is difficult.

Easy:

```text
p × q = n
```

Hard:

```text
n → p and q
```

---

# RSA Variables

| Variable | Meaning          |
| -------- | ---------------- |
| p        | First prime      |
| q        | Second prime     |
| n        | p × q            |
| e        | Public exponent  |
| d        | Private exponent |
| m        | Plaintext        |
| c        | Ciphertext       |

---

# RSA Key Generation

## Step 1

Choose:

```text
p and q
```

---

## Step 2

Calculate:

```text
n = p × q
```

---

## Step 3

Calculate:

```text
φ(n) = (p - 1)(q - 1)
```

---

## Step 4

Choose:

```text
e
```

where:

```text
gcd(e, φ(n)) = 1
```

---

## Step 5

Calculate:

```text
d
```

such that:

```text
e × d ≡ 1 mod φ(n)
```

---

# RSA Keys

Public Key:

```text
(n,e)
```

Private Key:

```text
(n,d)
```

---

# RSA Encryption

```text
c = m^e mod n
```

---

# RSA Decryption

```text
m = c^d mod n
```

---

# 5. Diffie-Hellman Key Exchange

Diffie-Hellman allows two parties to create a shared secret over an insecure channel.

---

## Public Values

```text
p = large prime
g = generator
```

---

## Private Values

Alice:

```text
a
```

Bob:

```text
b
```

---

## Public Keys

Alice:

```text
A = g^a mod p
```

Bob:

```text
B = g^b mod p
```

---

## Shared Secret

Alice:

```text
B^a mod p
```

Bob:

```text
A^b mod p
```

Both obtain:

```text
g^(ab) mod p
```

---

# Advantages

* Secure key agreement
* No pre-shared secret required

---

# Limitation

Diffie-Hellman does not authenticate users.

It is vulnerable to:

```text
Man-in-the-Middle Attack
```

unless combined with certificates or digital signatures.

---

# 6. SSH Authentication

SSH uses public key cryptography.

---

# Server Authentication

When connecting for the first time:

```bash
ssh user@host
```

SSH shows:

```text
The authenticity of host can't be established
```

You verify the server fingerprint.

Fingerprints are stored in:

```bash
~/.ssh/known_hosts
```

---

# Client Authentication

Two methods:

1. Password Authentication
2. Key Authentication

---

# Generate SSH Keys

```bash
ssh-keygen -t ed25519
```

Generated files:

```bash
id_ed25519
id_ed25519.pub
```

---

# Public Key

Safe to share.

---

# Private Key

Never share.

---

# Supported Algorithms

* RSA
* DSA
* ECDSA
* Ed25519

Recommended:

```text
Ed25519
```

---

# Authorized Keys

Trusted public keys are stored in:

```bash
~/.ssh/authorized_keys
```

---

# Key Permissions

Private keys must have:

```bash
chmod 600 id_ed25519
```

---

# Using a Private Key

```bash
ssh -i private_key user@host
```

---

# 7. Digital Signatures

Digital signatures prove:

* Authenticity
* Integrity

---

# Signing Process

Sender:

1. Create hash.
2. Encrypt hash using private key.

Result:

```text
Digital Signature
```

---

# Verification Process

Receiver:

1. Hash received document.
2. Decrypt signature using sender's public key.
3. Compare hashes.

If equal:

```text
Valid Signature
```

---

# Important Rule

```text
Sign with Private Key
Verify with Public Key
```

---

# Electronic Signature vs Digital Signature

Electronic Signature:

```text
Image pasted on document
```

Digital Signature:

```text
Cryptographic signature
```

Only digital signatures guarantee integrity.

---

# 8. Certificates and HTTPS

Certificates prove identity.

Example:

```text
https://tryhackme.com
```

---

# Certificate Contains

* Public Key
* Domain Name
* Organization Name
* Expiry Date
* CA Signature

---

# Certificate Authority (CA)

Trusted organizations that issue certificates.

Examples:

* DigiCert
* GlobalSign
* Let's Encrypt

---

# Chain of Trust

```text
Website Certificate
↓
Intermediate CA
↓
Root CA
↓
Browser Trust
```

---

# HTTPS Workflow

Browser:

1. Receives certificate.
2. Verifies signature.
3. Checks chain of trust.
4. Establishes secure connection.

---

# Free TLS Certificates

Use:

```text
Let's Encrypt
```

---

# 9. GPG / PGP

PGP = Pretty Good Privacy

GPG = GNU Privacy Guard

---

# Uses

* Email Encryption
* File Encryption
* Digital Signatures

---

# Generate Key Pair

```bash
gpg --full-gen-key
```

---

# Import Key

```bash
gpg --import file.key
```

---

# Decrypt File

```bash
gpg --decrypt message.gpg
```

---

# Public Key

Used for:

* Encryption
* Signature Verification

---

# Private Key

Used for:

* Decryption
* Signing

Never share it.

---

# GPG Workflow

Encrypt:

```text
Recipient Public Key
```

Decrypt:

```text
Recipient Private Key
```

Sign:

```text
Sender Private Key
```

Verify:

```text
Sender Public Key
```

---

# 10. Important Commands

## SSH

```bash
ssh-keygen -t ed25519
ssh-copy-id user@host
ssh -i keyfile user@host
```

---

## GPG

```bash
gpg --full-gen-key
gpg --list-keys
gpg --list-secret-keys
gpg --import keyfile
gpg --decrypt file.gpg
gpg --sign file
```

---

# 11. Quick Revision

```text
Confidentiality → Encryption
Integrity → Hashing
Authenticity → Digital Signature
Authentication → Identity Verification

Public Key → Share
Private Key → Keep Secret

Encrypt → Public Key
Decrypt → Private Key

Sign → Private Key
Verify → Public Key
```

# Key Takeaway

> Public key cryptography enables secure communication, authentication, integrity verification, digital signatures, and secure key exchange over insecure networks.
