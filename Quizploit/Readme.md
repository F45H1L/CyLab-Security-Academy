# Quizploit

**Category:** Binary Exploitation
**Difficulty:** Easy
**Platform:** Cylab Academy

## Challenge Description

Quizploit is a binary analysis challenge where the objective is to analyze a C source file and its corresponding ELF binary to answer 13 questions correctly.

The challenge focuses on basic ELF analysis, binary protections, buffer overflows, and exploitation concepts.

## Files

* `vuln.c` — C source code
* `vuln` — Compiled ELF binary

## Source Code Analysis

The vulnerable function is:

```c
void vuln(){
    char buffer[0x15] = {0};
    fprintf(stdout, "\nEnter payload: ");
    fgets(buffer, 0x90, stdin);
}
```

The buffer is only `0x15` bytes (21 bytes), while `fgets()` can read up to `0x90` bytes (144 bytes).

This creates a **stack-based buffer overflow vulnerability**.

The source also contains a `win()` function:

```c
void win(){
    system("cat flag.txt");
}
```

However, `win()` is never called by the program normally.

## Binary Analysis

Run the following commands:

```bash
file vuln
```

This identifies the binary architecture and linking information.

Check the binary protections with:

```bash
checksec --file=vuln
```

Useful tools for further analysis:

```bash
gdb ./vuln
```

```bash
objdump -d vuln
```

```bash
nm vuln
```

## Quiz Answers

The 13 questions were answered as follows:

| Question                         | Answer            |
| -------------------------------- | ----------------- |
| 1. ELF architecture              | `64-bit`          |
| 2. Linking                       | `dynamic`         |
| 3. Stripped status               | `not stripped`    |
| 4. Buffer size                   | `0x15`            |
| 5. Bytes read by `fgets()`       | `0x90`            |
| 6. Buffer overflow vulnerability | `yes`             |
| 7. Vulnerable C function         | `fgets`           |
| 8. Uncalled function             | `win`             |
| 9. Exploitation technique        | `buffer overflow` |
| 10. Possible overflow            | `0x7b`            |
| 11. Enabled protection           | `NX`              |
| 12. Technique to bypass NX       | `ROP`             |
| 13. `win()` address              | `0x401176`        |

### Overflow Calculation

The amount by which the input can exceed the declared buffer size is:

```text
0x90 - 0x15 = 0x7b
```

Therefore, the possible overflow is **`0x7b` bytes**.

## Exploitation Concept

Because the binary has NX enabled, injecting and executing shellcode directly on the stack is prevented.

A suitable technique is **Return-Oriented Programming (ROP)**, where existing executable code in the binary is reused.

The target function is:

```text
win() = 0x401176
```

Conceptually, the payload can overwrite the saved return address with the address of `win()`:

```text
padding + address of win()
```

The `win()` function executes:

```c
system("cat flag.txt");
```

which displays the flag.

## Flag

```text
academy{my_bIn@4y_3xpl0it_fL@g_95fef02a}
```

## Key Takeaways

* ELF binaries can be analyzed using `file`, `checksec`, `gdb`, `objdump`, and `nm`.
* `fgets()` is not automatically safe; the maximum read size must match the destination buffer.
* A mismatch between buffer size and input size can result in a stack buffer overflow.
* NX prevents execution of injected code on the stack.
* ROP can reuse existing executable instructions when NX is enabled.
* Unused functions such as `win()` are common targets in beginner ret2win-style challenges.