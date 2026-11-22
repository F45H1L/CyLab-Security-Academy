# picoCTF 2022 – Glitch Cat

## Challenge Information

| Field | Details |
|---|---|
| Challenge | Glitch Cat |
| Category | General Skills |
| Difficulty | Easy |
| Author | LT 'syreal' Jones |
| Event | picoMini 2022 |

## Objective

The flag printing service is glitching and displays valid Python code instead of printing the actual flag.

The objective is to understand the generated Python expression and decode it to retrieve the flag.

## Step 1: Connect to the Instance

Use Netcat (`nc`) to connect to the challenge server.

```bash
nc xebec.cylabacademy.net 12755
```

## Step 2: Analyze the Output

The server returns the following Python expression:

```python
'academy{gl17ch_m3_n07_' + chr(0x33) + chr(0x38) + chr(0x38) + chr(0x31) + chr(0x64) + chr(0x63) + chr(0x65) + chr(0x33) + '}'
```

The output looks unusual, but it is valid Python code.

The `chr()` function converts an integer representing a Unicode code point into its corresponding character.

The values are provided in hexadecimal format (`0x`).

## Step 3: Decode the Hexadecimal Values

Convert each hexadecimal value into its corresponding ASCII character.

| Hexadecimal | Decimal | Character |
|---|---:|---|
| 0x33 | 51 | 3 |
| 0x38 | 56 | 8 |
| 0x38 | 56 | 8 |
| 0x31 | 49 | 1 |
| 0x64 | 100 | d |
| 0x63 | 99 | c |
| 0x65 | 101 | e |
| 0x33 | 51 | 3 |

The decoded characters are:

```text
3881dce3
```

## Step 4: Execute the Python Expression

Instead of manually decoding each character, execute the entire expression using Python 3.

```bash
python3 -c "print('academy{gl17ch_m3_n07_' + chr(0x33) + chr(0x38) + chr(0x38) + chr(0x31) + chr(0x64) + chr(0x63) + chr(0x65) + chr(0x33) + '}')"
```

The program prints the complete flag.

## Flag

```text
academy{gl17ch_m3_n07_3881dce3}
```

## Key Learnings

- **ASCII:** A character encoding standard that maps characters to numeric values.
- **Hexadecimal:** A base-16 number system commonly used in programming.
- **Python `chr()`:** Converts a Unicode code point into its corresponding character.
- **Python `ord()`:** Converts a character into its Unicode code point.
- **Netcat (`nc`):** A command-line networking utility used to connect to remote services.

## Conclusion

The challenge demonstrates how seemingly corrupted output can actually be valid Python code. By recognizing the `chr()` function and converting the hexadecimal values into characters, we can reconstruct the original flag.