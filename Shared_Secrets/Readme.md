# Shared Secrets

## Overview

**Category:** Cryptography  
**Difficulty:** Easy  
**Challenge:** Shared Secrets

### Description

A message was encrypted using a Diffie–Hellman shared secret. The challenge provides the public parameters, the server's public key, the client's private key, and an encrypted flag.

The objective is to recover the shared secret, derive the XOR encryption key, and decrypt the flag.

## Challenge Information

The challenge provides the following values:

- `g`: Diffie–Hellman generator
- `p`: Large prime modulus
- `A`: Server's public key
- `b`: Client's private key (leaked)
- `enc`: Hexadecimal ciphertext

The encryption code is:

```python
shared = pow(A, b, p)
enc = bytes([x ^ (shared % 256) for x in flag])
```

## Tools Required

- Python 3
- A terminal (Kali Linux, Linux, or Windows)
- Basic knowledge of modular exponentiation and XOR encryption

## Step-by-Step Solution

### Step 1: Analyze the challenge

The challenge uses the Diffie–Hellman key exchange algorithm to calculate a shared secret.

Normally, the server's private key `a` and the client's private key `b` should remain confidential. However, the challenge exposes `b`, allowing us to calculate the shared secret directly.

The server's public key is defined as:

```python
A = pow(g, a, p)
```

The shared secret is calculated as:

```python
shared = pow(A, b, p)
```

Since `A`, `b`, and `p` are available, there is no need to recover the server's private key.

### Step 2: Understand the encryption

The flag is encrypted by XORing each byte with the lowest byte of the shared secret:

```python
key = shared % 256
```

The encryption operation is:

```python
cipher_byte = flag_byte ^ key
```

XOR is reversible. Applying the same key to the ciphertext recovers the original plaintext:

```python
flag_byte = cipher_byte ^ key
```

### Step 3: Create the decryption script

Create a Python file named `solve.py` and insert the challenge values for `A`, `p`, `b`, and `enc`.

```python
# Shared Secrets — Cryptography Easy

A = 573064858108775587796401031506157877018348288426226470285524205679136075971072753567297355763163615870402991691187850746336169042262692610483425859830970803563824285074033597222769345556533666344979747486571381174509140455398567155637545070339870299749606406694126580709457390475875189904723414950764573551900114879

p = 2927273144810939766701580090813587829846998218139050366191650895496341639672151115600622801622875986429173669666450605563086095565809406776099318149676703864803530494465478726922189453273460829331935414785533102652757649572940785971419755291375295328412738229475603220295990344218551730029944293124696567146516090193

b = 310480984845844782350469929488957103404065672967681721794433968598078606678457574115948893342658016388850300951548689390362586285081923158250912861928954606683971373959652287246659410807804603053375492104235114782698335962292773627028417650732575398422905666599383548621368163602765974099546321049636858616564366046

enc = bytes.fromhex(
    "43414346474f5b59464a7d5111415011567d16161347401b47475f"
)

# Calculate the shared secret
shared = pow(A, b, p)

# Extract the single-byte XOR key
key = shared % 256

# Decrypt the ciphertext
flag = bytes(byte ^ key for byte in enc)

print("[+] Shared secret calculated")
print(f"[+] XOR key: {key}")
print(f"[+] Flag: {flag.decode()}")
```

### Step 4: Execute the script

Run the following command from the directory containing `solve.py`:

```bash
python3 solve.py
```

The script calculates the shared secret, extracts the XOR key, and decrypts the ciphertext.

### Step 5: Recover the flag

The decrypted output should have the format:

```text
academy{...}
```

Submit the recovered flag to the challenge platform.

## Root Cause

The challenge contains two major cryptographic weaknesses:

1. **Private key disclosure:** The client's private key `b` is exposed. Anyone with this value and the server's public key can calculate the shared secret.
2. **Weak encryption construction:** The program uses a single-byte XOR key repeatedly across the entire message. This does not provide secure encryption.

## Lessons Learned

- Private keys must never be exposed in public challenge transcripts or production systems.
- Diffie–Hellman relies on keeping private exponents secret.
- XOR encryption with a repeated single-byte key is not suitable for protecting sensitive information.
- Use established cryptographic libraries and authenticated encryption algorithms, such as AES-GCM or ChaCha20-Poly1305, for real applications.

## Conclusion

The challenge is solved by exploiting the leaked client private key to calculate the Diffie–Hellman shared secret. Taking `shared % 256` reveals the XOR key, which can then be applied to every ciphertext byte to recover the flag.

**Vulnerability:** Exposed cryptographic private key and insecure XOR-based encryption.