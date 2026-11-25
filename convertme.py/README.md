# convertme.py

**Category:** General Skills  
**Difficulty:** Easy  
**Event:** Beginner picoMini 2022  
**Author:** LT 'syreal' Jones

## Challenge Description

Run the provided Python script and convert the given decimal number into binary to obtain the flag.

## Solution

### Step 1: Download the Python Script

Download the challenge file using `wget`:

```bash
wget https://challenge-files.cylabacademy.net/library/d507213597114cdf0943d7f7c585eab624e16586a7cdcb88229fd24c143128a1/convertme.py
```

### Step 2: Execute the Script

Run the Python script:

```bash
python3 convertme.py
```

The script displays a decimal number and asks for its binary equivalent.

**Challenge Output:**

```text
If 97 is in decimal base, what is it in binary base?
```

### Step 3: Convert Decimal to Binary

Python provides a built-in `bin()` function to convert decimal integers into binary.

Execute:

```bash
python3 -c "print(bin(97)[2:])"
```

**Output:**

```text
1100001
```

Alternatively, the conversion can be performed using Python:

```python
decimal_number = 97
binary_number = bin(decimal_number)[2:]
print(binary_number)
```

### Step 4: Submit the Answer

Enter the binary value into the running challenge:

```text
1100001
```

The answer is correct, and the script reveals the flag.

## Flag

```text
academy{4ll_y0ur_b4535_fff0af3d}
```

## Key Learnings

- Understood decimal-to-binary number conversion.
- Learned to use Python's built-in `bin()` function.
- Learned how `[2:]` removes the `0b` prefix from binary output.
- Practiced executing Python scripts in the Kali Linux terminal.