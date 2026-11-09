# GDB Baby Steps (1–4)

Write-ups for four beginner GDB reverse-engineering challenges.

> **Environment:** Kali Linux, GDB, x86-64 binaries  
> **Note:** These instructions are for the provided challenge binaries.

---

## 1. GDB baby step 1

**Goal:** Find the value of `eax` at the end of `main`, in decimal.

### Download and prepare

```bash
wget https://challenge-files.cylabacademy.net/library/03b038548e2a34973bde05a78fadc1f09467a1d03ffbfb9e4b48bd44e87c0e70/debugger0_a
chmod +x debugger0_a
gdb ./debugger0_a
```

### Disassemble `main`

At the GDB prompt:

```gdb
disassemble main
```

Relevant instruction:

```asm
mov $0x86342,%eax
```

This loads `0x86342` into `eax`. No later instruction changes it before `main` returns.

Convert hexadecimal to decimal:

```bash
python3 -c "print(0x86342)"
```

Output:

```text
549698
```

**Flag:**

```text
academy{549698}
```

---

## 2. GDB baby step 2

**Goal:** Determine the final `eax` value after a loop executes in `main`.

### Download and prepare

```bash
wget https://challenge-files.cylabacademy.net/library/eea5e81c4ce7d6dcb85a3fe7d2edae4465705d230c68c45365cb9f24a69dfcf1/debugger0_b
chmod +x debugger0_b
gdb ./debugger0_b
```

### Disassemble `main`

```gdb
disassemble main
```

Important instructions:

```asm
movl $0x1e0da,-0x4(%rbp)   # initial value
movl $0x25f,-0xc(%rbp)      # loop limit: 607
movl $0x0,-0x8(%rbp)        # i = 0

mov -0x8(%rbp),%eax
add %eax,-0x4(%rbp)          # add i to initial value
addl $0x1,-0x8(%rbp)        # i++
```

The loop runs while `i < 607`, so it adds every integer from `0` through `606`.

### Inspect `eax` with GDB

The `ret` instruction is at `0x401142`. Break there:

```gdb
break *0x401142
run
print/d $eax
```

GDB output:

```text
$1 = 307019
```

**Flag:**

```text
academy{307019}
```

---

## 3. GDB baby step 3

**Goal:** Examine the four bytes of a constant as they are stored in memory.

### Download and prepare

```bash
wget https://challenge-files.cylabacademy.net/library/5eb60afe8ad6df789decfd9d5af027dc8184bfbc20e953f76ed38bd9ce3d9582/debugger0_c
chmod +x debugger0_c
gdb ./debugger0_c
```

### Disassemble `main`

```gdb
disassemble main
```

Relevant instructions:

```asm
movl $0x2262c96b,-0x4(%rbp)
mov -0x4(%rbp),%eax
```

The constant is stored at `$rbp-4`. The instruction immediately after the store is at `main+22`.

### Break after the store and examine memory

```gdb
break *main+22
run
x/4xb $rbp-4
```

GDB output:

```text
0x7fffffffdcdc: 0x6b  0xc9  0x62  0x22
```

### Understand little-endian order

On x86-64, values are stored in **little-endian** byte order: the least significant byte comes first.

The original constant is:

```text
0x2262c96b
```

The bytes in memory are:

```text
6b c9 62 22
```

Combine them in the displayed memory order:

**Flag:**

```text
academy{0x6bc96222}
```

---

## 4. GDB baby step 4

**Goal:** Find the constant used to multiply `eax` in a function called by `main`.

### Download and prepare

```bash
wget https://challenge-files.cylabacademy.net/library/5c4fe0af7766d02123d4ebea4a32b3b461fc6c5947049092d61d47518f298068/debugger0_d
chmod +x debugger0_d
gdb ./debugger0_d
```

### Disassemble `main`

```gdb
disassemble main
```

The relevant call is:

```asm
call 0x401106 <func1>
```

### Disassemble the called function

```gdb
disassemble func1
```

Relevant instruction:

```asm
imul $0x3269,%eax,%eax
```

The multiplication constant is `0x3269`.

Convert it to decimal:

```bash
python3 -c "print(0x3269)"
```

Output:

```text
12905
```

**Flag:**

```text
academy{12905}
```

---

## Quick reference

| Challenge | Technique | Flag |
|---|---|---|
| GDB baby step 1 | Read immediate value assigned to `eax` | `academy{549698}` |
| GDB baby step 2 | Break at `ret` and inspect `eax` | `academy{307019}` |
| GDB baby step 3 | Examine memory bytes; account for little-endian | `academy{0x6bc96222}` |
| GDB baby step 4 | Disassemble called function; identify `imul` constant | `academy{12905}` |

## Useful GDB commands

```gdb
disassemble main       # Disassemble main
disassemble func1     # Disassemble a named function
break *0xADDRESS      # Break at a specific instruction address
break *main+OFFSET    # Break at an offset from main
run                   # Start/restart the program
print/d $eax           # Print eax in decimal
print/x $eax           # Print eax in hexadecimal
x/4xb ADDRESS          # Examine 4 bytes in hexadecimal
info registers         # Display registers
```

### Notes

- Replace example addresses and offsets with those shown by your own disassembly.
- `chmod +x` makes a downloaded binary executable.
- Use `quit` to exit GDB.
- The answers above are based on the GDB output obtained while solving these challenge binaries.