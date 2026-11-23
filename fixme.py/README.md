# fixme.py

## Challenge 1: fixme1.py

**Category:** General Skills  
**Difficulty:** Easy  
**Objective:** Fix the syntax error in the Python script to print the flag.

### Problem

The Python script contained an indentation error in the final `print()` statement.

### Solution

The `print()` statement was incorrectly indented. Since it was outside the function and loop, it needed to start at the beginning of the line.

**Incorrect code:**
```python
flag = str_xor(flag_enc, 'enkidu')
    print('That is correct! Here\'s your flag: ' + flag)
```

**Corrected code:**
```python
flag = str_xor(flag_enc, 'enkidu')
print('That is correct! Here\'s your flag: ' + flag)
```

### Execution

Run the script using:

```bash
python3 fixme1.py
```

### Flag

```text
academy{1nd3nt1ty_cr1515_f63757ad}
```

### Key Takeaway

Python uses indentation to define code blocks. Incorrect indentation can cause an `IndentationError` and prevent the script from executing.

---

## Challenge 2: fixme2.py

**Category:** General Skills  
**Difficulty:** Easy  
**Objective:** Fix the syntax error in the Python script to print the flag.

### Problem

The Python script contained an incorrect operator in an `if` condition.

### Solution

The original code used a single equal sign (`=`) instead of the equality comparison operator (`==`).

**Incorrect code:**
```python
if flag = "":
    print('String XOR encountered a problem, quitting.')
```

**Corrected code:**
```python
if flag == "":
    print('String XOR encountered a problem, quitting.')
```

### Execution

Run the script using:

```bash
python3 fixme2.py
```

### Flag

```text
academy{3qu4l1ty_n0t_4551gnm3nt_ad29eab9}
```

### Key Takeaway

- `=` is used for variable assignment.
- `==` is used to compare two values.
- Python requires `==` when checking equality in conditional statements.

---

## Summary

| Challenge | Vulnerability / Error | Fix |
|---|---|---|
| fixme1.py | Incorrect indentation | Correct indentation |
| fixme2.py | Assignment instead of equality | Replace `=` with `==` |

**Skills Learned:**
- Python syntax and indentation
- Conditional statements (`if/else`)
- Assignment and equality operators
- Debugging Python scripts