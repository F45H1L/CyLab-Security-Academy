# PW Crack

---

## 1. PW Crack 1

**Difficulty:** Easy  
**Category:** General Skills

### Objective
Find the correct password in the provided Python password checker and decrypt the encrypted flag.

### Files
- `level1.py` – Password checker
- `level1.flag.txt.enc` – Encrypted flag

### Analysis

Open the Python file:

```bash
nano level1.py
```

Inspecting the `level_1_pw_check()` function reveals:

```python
def level_1_pw_check():
    user_pw = input("Please enter correct password for flag: ")
    if(user_pw == "8713"):
        print("Welcome back... your flag, user:")
        decryption = str_xor(flag_enc.decode(), user_pw)
        print(decryption)
        return
    print("That password is incorrect")
```

The password is directly hardcoded in the source code.

### Solution

**Password:**
```text
8713
```

Run the script:

```bash
python3 level1.py
```

Enter the password when prompted.

### Flag

```text
academy{545h_r1ng1ng_d218163c}
```

### Key Learning

- Identifying hardcoded credentials.
- Inspecting Python source code.
- Understanding how XOR is used for decryption.
- Never store passwords directly in source code.

---

## 2. PW Crack 2

**Difficulty:** Easy  
**Category:** General Skills

### Objective
Find the hidden password by analyzing the encoding used in the Python password checker.

### Files
- `level2.py` – Password checker
- `level2.flag.txt.enc` – Encrypted flag

### Analysis

Open the Python file:

```bash
nano level2.py
```

Inspect the `level_2_pw_check()` function:

```python
def level_2_pw_check():
    user_pw = input("Please enter correct password for flag: ")
    if(user_pw == chr(0x33) + chr(0x39) + chr(0x63) + chr(0x65)):
        print("Welcome back... your flag, user:")
        decryption = str_xor(flag_enc.decode(), user_pw)
        print(decryption)
        return
    print("That password is incorrect")
```

Unlike PW Crack 1, the password is represented using hexadecimal ASCII values.

### Password Decoding

The `chr()` function converts an integer into its corresponding character.

| Hexadecimal | Decimal | Character |
|---|---:|---|
| `0x33` | 51 | 3 |
| `0x39` | 57 | 9 |
| `0x63` | 99 | c |
| `0x65` | 101 | e |

Combining the characters gives:

**Password:**
```text
39ce
```

### Solution

Run the script:

```bash
python3 level2.py
```

Enter the decoded password when prompted.

### Flag

```text
academy{tr45h_51ng1ng_82234aa4}
```

### Key Learning

- Understanding ASCII encoding.
- Recognizing hexadecimal values.
- Using Python's `chr()` function.
- Understanding why encoding is not encryption.

---

## Summary

| Challenge | Password | Technique |
|---|---|---|
| PW Crack 1 | `8713` | Hardcoded password |
| PW Crack 2 | `39ce` | ASCII hexadecimal encoding |

### Important Python Functions

| Function | Purpose |
|---|---|
| `chr()` | Converts an integer into a character |
| `ord()` | Converts a character into its Unicode code point |
| `.decode()` | Converts bytes into a string using an encoding |

### Final Takeaways

1. Always inspect source code when analyzing password checkers.
2. Hardcoded credentials can expose sensitive information.
3. Hexadecimal ASCII values can easily be converted back into readable text.
4. Encoding is not a security mechanism.
5. Understanding the code is more useful than blindly guessing passwords.

**Status:** Both challenges successfully solved.