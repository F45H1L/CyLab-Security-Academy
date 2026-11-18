# ASCII FTW

**Platform:** Cylab Academy  
**Category:** Reverse Engineering  
**Difficulty:** Easy

## Challenge Description

The program constructs the flag using hexadecimal ASCII values. The objective is to disassemble the executable and identify the flag text.

## Tools Used

- `objdump` – Disassemble the executable
- `grep` – Extract hexadecimal values
- `sed` – Remove hexadecimal prefixes
- `tr` – Remove newlines
- `xxd` – Convert hexadecimal bytes into ASCII

## Solution

### 1. Identify the binary

```bash
file asciiftw
```

### 2. Disassemble the executable

```bash
objdump -d asciiftw
```

The disassembly contains `movb` instructions that store individual hexadecimal ASCII values into memory.

### 3. Extract and decode the hexadecimal values

```bash
objdump -d asciiftw | grep movb | grep -oE '\\$0x[0-9a-f]{2}' | sed 's/\\$0x//' | tr -d '\\n' | xxd -r -p
```

### Explanation

- `objdump -d asciiftw` – Disassembles the executable.
- `grep movb` – Filters instructions that store individual bytes.
- `grep -oE '\\$0x[0-9a-f]{2}'` – Extracts hexadecimal byte values.
- `sed 's/\\$0x//'` – Removes the `$0x` prefix.
- `tr -d '\\n'` – Joins all hexadecimal values into a single string.
- `xxd -r -p` – Converts the hexadecimal string into ASCII text.

### 4. Flag

```text
academy{ASCII_IS_EASY_6A73FC58}
```

## Key Takeaways

- Learned how to inspect ELF executables using `objdump`.
- Identified ASCII characters represented as hexadecimal values.
- Extracted byte values from assembly instructions.
- Automated hexadecimal-to-ASCII conversion using Linux command-line utilities.

## Status

**Challenge Completed Successfully!**