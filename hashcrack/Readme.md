# hashcrack

**Category:** Cryptography  
**Difficulty:** Easy  
**Challenge Type:** 

## 1. Challenge Description

A company stored a secret message on a server, but the administrator used weakly hashed passwords. The objective is to identify the hash algorithms, crack the passwords, and retrieve the secret flag from the remote server.

**Challenge server:**

```bash
nc xebec.cylabacademy.net 23527
```

## 2. Objective

- Connect to the remote challenge server.
- Identify each hash algorithm.
- Crack the hashes using Hashcat and a password dictionary.
- Submit the recovered passwords to the server.
- Retrieve the final flag.

## 3. Tools Used

- **Kali Linux** — penetration testing environment.
- **Netcat (`nc`)** — connect to the remote challenge service.
- **Hashcat** — password hash recovery tool.
- **RockYou wordlist** — dictionary of commonly used passwords.
- **Nano** — create and edit hash files.

## 4. Initial Connection

Connect to the challenge server:

```bash
nc xebec.cylabacademy.net 23527
```

The server presents the first hash and requests its corresponding password.

## 5. Solution Walkthrough

### Stage 1: Crack the MD5 Hash

**Hash provided:**

```text
482c811da5d5b4bc6d497ffa98491e38
```

The hash contains 32 hexadecimal characters, which is consistent with MD5.

Create a file containing the hash:

```bash
echo '482c811da5d5b4bc6d497ffa98491e38' > hash.txt
```

Run Hashcat using mode `0` for MD5:

```bash
hashcat -m 0 hash.txt /usr/share/wordlists/rockyou.txt
```

Display the recovered password:

```bash
hashcat -m 0 hash.txt /usr/share/wordlists/rockyou.txt --show
```

**Recovered password:**

```text
password123
```

Submit `password123` to the running Netcat session.

The server confirms that the MD5 hash has been cracked and provides the next hash.

### Stage 2: Crack the SHA-1 Hash

**Hash provided:**

```text
b7a875fc1ea228b9061041b7cec4bd3c52ab3ce3
```

The hash contains 40 hexadecimal characters, which is consistent with SHA-1.

Save the hash:

```bash
echo 'b7a875fc1ea228b9061041b7cec4bd3c52ab3ce3' > hash.txt
```

Run Hashcat using mode `100` for SHA-1:

```bash
hashcat -m 100 hash.txt /usr/share/wordlists/rockyou.txt
```

Display the recovered password:

```bash
hashcat -m 100 hash.txt /usr/share/wordlists/rockyou.txt --show
```

**Recovered password:**

```text
letmein
```

Submit `letmein` to the running Netcat session.

The server confirms that the SHA-1 hash has been cracked and provides the third hash.

### Stage 3: Crack the SHA-256 Hash

**Hash provided:**

```text
916e8c4f79b25028c9e467f1eb8eee6d6bbdff965f9928310ad30a8d88697745
```

The hash contains 64 hexadecimal characters, which is consistent with SHA-256.

Save the hash:

```bash
echo '916e8c4f79b25028c9e467f1eb8eee6d6bbdff965f9928310ad30a8d88697745' > hash.txt
```

Run Hashcat using mode `1400` for raw SHA-256:

```bash
hashcat -m 1400 hash.txt /usr/share/wordlists/rockyou.txt
```

Display the recovered password:

```bash
hashcat -m 1400 hash.txt /usr/share/wordlists/rockyou.txt --show
```

**Recovered password:**

```text
qwerty098
```

Submit `qwerty098` to the running Netcat session.

The server confirms that the SHA-256 hash has been cracked and reveals the flag.

## 6. Results Summary

| Stage | Algorithm | Hashcat Mode | Recovered Password |
|---|---|---:|---|
| 1 | MD5 | `0` | `password123` |
| 2 | SHA-1 | `100` | `letmein` |
| 3 | SHA-256 | `1400` | `qwerty098` |

## 7. Flag

```text
academy{UseStr0nG_h@shEs_&PaSswDs!_062c2b77}
```

## 8. Understanding the Attack

A password hash is a one-way transformation of input data. A dictionary attack works by hashing candidate passwords and comparing the results against the target hash.

In this challenge:

1. The server supplied a hash.
2. The hash's length provided an initial clue about its algorithm.
3. Hashcat tested candidate passwords from the RockYou dictionary.
4. When a candidate produced the matching hash, Hashcat recovered the plaintext password.
5. The password was submitted to the server to advance to the next stage.

The three hashes were cracked quickly because their corresponding passwords appeared in the dictionary.

## 9. Security Lessons

- **Avoid weak passwords:** Common passwords such as `password123`, `letmein`, and `qwerty098` are vulnerable to dictionary attacks.
- **Avoid MD5 and SHA-1 for password storage:** These algorithms are unsuitable for modern password hashing.
- **Do not use raw SHA-256 for passwords:** Although SHA-256 is a secure cryptographic hash function for many applications, it is designed to be fast, which makes large-scale password guessing easier.
- **Use dedicated password hashing algorithms:** Argon2id is a recommended choice; scrypt and bcrypt are also widely used alternatives.
- **Use unique salts:** A unique random salt for every password prevents identical passwords from producing identical stored hashes and reduces the effectiveness of precomputed hash tables.
- **Use defense in depth:** Rate limiting, secure authentication, multi-factor authentication, and appropriate password policies help reduce the risk of account compromise.

## 10. Conclusion

The `hashcrack` challenge demonstrated how to identify common hash algorithms and recover weak passwords using Hashcat and a dictionary wordlist.

By successfully cracking MD5, SHA-1, and SHA-256 hashes, the challenge illustrated why weak passwords and unsuitable password-storage mechanisms can expose sensitive information.

**Key takeaway:** Secure password storage requires strong password choices and a purpose-built, salted password hashing algorithm with an appropriate work factor.