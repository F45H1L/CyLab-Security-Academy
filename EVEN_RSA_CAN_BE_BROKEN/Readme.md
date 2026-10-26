# EVEN RSA CAN BE BROKEN???

## Challenge Information

- **Category:** Cryptography
- **Difficulty:** Easy
- **Challenge:** EVEN RSA CAN BE BROKEN???
- **Remote Service:** `chatelaine.cylabacademy.net:<TARGET-PORT>`
- **Public Exponent:** `65537`

## Objective

The challenge provides an RSA-encrypted flag and asks us to decrypt it using the public modulus \(N\) and public exponent \(e\).

The hints point toward weak randomness and comparing RSA moduli across multiple requests.

## Step 1: Connect to the Service

Connect to the challenge using Netcat:

```bash
nc chatelaine.cylabacademy.net <TARGET-PORT>
```

The service returns the RSA modulus, public exponent, and ciphertext.

Example output:

```text
N: 17114156479182441748325237544587977510113769896383073025026649083088570774604979010131715106540062947172493638768304585002645503393897374073204999271148974

e: 65537

cyphertext: 12254619905935614091856720382458481865574336634124476327423301218959945788086875582171833079981104594297758012337968513135746525299139733787884297205964357
```

## Step 2: Analyze the RSA Modulus

RSA normally generates its modulus as:

\[
N=pq
\]

where \(p\) and \(q\) are large prime numbers.

However, inspecting the supplied modulus reveals that it is **even**.

An even integer is divisible by \(2\), so:

\[
p=2,\qquad q=\frac{N}{2}
\]

This immediately exposes a factor of the RSA modulus and avoids the need for conventional RSA factorization.

**Important:** This explains the vulnerability in the observed sample. The challenge hints also suggest investigating weak randomness and shared factors across multiple requests.

## Step 3: Calculate the Private Key

Once the modulus is factored, calculate Euler's totient:

\[
\phi(N)=(p-1)(q-1)
\]

Then compute the private exponent:

\[
d=e^{-1}\pmod{\phi(N)}
\]

In Python, the modular inverse can be calculated with:

```python
d = pow(e, -1, phi)
```

## Step 4: Decrypt the Ciphertext

RSA decryption is performed using:

\[
m=c^d\bmod N
\]

The resulting integer \(m\) represents the original message. Convert it into bytes to recover the flag.

Create a file named `solve.py`:

```python
from Crypto.Util.number import long_to_bytes

N = 17114156479182441748325237544587977510113769896383073025026649083088570774604979010131715106540062947172493638768304585002645503393897374073204999271148974

e = 65537

c = 12254619905935614091856720382458481865574336634124476327423301218959945788086875582171833079981104594297758012337968513135746525299139733787884297205964357

# Factor the even modulus
p = 2
q = N // p

# Calculate Euler's totient
phi = (p - 1) * (q - 1)

# Recover the private exponent
d = pow(e, -1, phi)

# Decrypt the ciphertext
m = pow(c, d, N)

# Convert the integer to bytes
flag = long_to_bytes(m).decode()

print("p =", p)
print("q =", q)
print("d =", d)
print("Flag:", flag)
```

Run the script:

```bash
python3 solve.py
```

If the required library is missing, install PyCryptodome:

```bash
python3 -m pip install pycryptodome
```

## Step 5: Recover the Flag

The script successfully decrypts the supplied ciphertext and produces:

```text
Flag: academy{tw0_1$_pr!m3d832b35c}
```

**Recovered flag:**

`academy{tw0_1$_pr!m3d832b35c}`

## Why the Attack Works

RSA relies on the difficulty of factoring a modulus constructed from two large prime numbers. In this instance, the modulus is even, which immediately reveals the factor \(2\).

Knowing both factors allows us to calculate the totient and recover the private exponent. We can then decrypt the ciphertext using modular exponentiation.

The challenge's additional hints suggest that weak randomness or shared prime factors between different RSA moduli may be another intended avenue of investigation. To confirm that attack path, collect multiple moduli and check their pairwise greatest common divisors.

## Key Takeaways

- Inspect RSA public parameters before attempting expensive attacks.
- A modulus that is even has an immediately discoverable factor.
- Knowing the prime factors of \(N\) enables calculation of the private exponent.
- Weak random-number generation can compromise RSA keys.
- Comparing multiple RSA moduli using GCD can reveal shared prime factors.
- RSA security depends on correct key generation and implementation, not just a large key size.

## Conclusion

The challenge was solved by identifying that the supplied RSA modulus was even, factoring it using \(2\), calculating the private exponent, and decrypting the ciphertext.

This demonstrates how a fundamental key-generation error can undermine RSA security without requiring a break of the RSA cryptographic algorithm itself.