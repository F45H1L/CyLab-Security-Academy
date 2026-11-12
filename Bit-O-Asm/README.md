# Bit-O-Asm (1–4)

Solutions for the Cylab Academy **Bit-O-Asm** challenges by LT 'syreal' Jones.

## Bit-O-Asm-1

**Goal:** Find the value in `eax` and submit it in decimal as `academy{n}`.

### Assembly

```asm
mov eax,0x30
```

### Explanation

- `mov` copies a value into a register.
- `0x30` is hexadecimal, which equals decimal **48**.
- The instruction directly places `48` in `eax`.

**Flag:** `academy{48}`

---

## Bit-O-Asm-2

### Assembly

```asm
mov DWORD PTR [rbp-0x4],0x9fe1a
mov eax,DWORD PTR [rbp-0x4]
```

### Explanation

1. `DWORD PTR [rbp-0x4]` refers to a 4-byte value stored at a memory location relative to `rbp`.
2. The first instruction stores `0x9fe1a` at that location.
3. The second instruction loads that stored value into `eax`.
4. Convert hexadecimal to decimal:

   `0x9FE1A` = **654874**

**Flag:** `academy{654874}`

---

## Bit-O-Asm-3

### Assembly

```asm
mov DWORD PTR [rbp-0xc],0x9fe1a
mov DWORD PTR [rbp-0x8],0x4
mov eax,DWORD PTR [rbp-0xc]
imul eax,DWORD PTR [rbp-0x8]
add eax,0x1f5
mov DWORD PTR [rbp-0x4],eax
mov eax,DWORD PTR [rbp-0x4]
```

### Explanation

1. Store `0x9FE1A` (654874) at `[rbp-0xc]`.
2. Store `0x4` (4) at `[rbp-0x8]`.
3. Load 654874 into `eax`.
4. `imul` multiplies `eax` by 4:

   `654874 × 4 = 2619496`

5. `add eax,0x1f5` adds `0x1F5` (501):

   `2619496 + 501 = 2619997`

6. The result is stored in `[rbp-0x4]` and loaded back into `eax`.

**Flag:** `academy{2619997}`

---

## Bit-O-Asm-4

### Assembly

```asm
mov DWORD PTR [rbp-0x4],0x9fe1a
cmp DWORD PTR [rbp-0x4],0x2710
jle <main+37>
sub DWORD PTR [rbp-0x4],0x65
jmp <main+41>
add DWORD PTR [rbp-0x4],0x65
mov eax,DWORD PTR [rbp-0x4]
```

### Explanation

1. Store `0x9FE1A` (654874) at `[rbp-0x4]`.
2. `cmp` compares 654874 with `0x2710` (10000).
3. `jle` means “jump if less than or equal” using a signed comparison. Since 654874 is greater than 10000, the jump is **not taken**.
4. Execute `sub ... 0x65`: subtract `0x65` (101):

   `654874 − 101 = 654773`

5. `jmp <main+41>` skips the `add` instruction.
6. The final `mov` loads 654773 into `eax`.

**Flag:** `academy{654773}`

---

## Final Answers

| Challenge | Decimal value in `eax` | Flag |
|---|---:|---|
| Bit-O-Asm-1 | 48 | `academy{48}` |
| Bit-O-Asm-2 | 654874 | `academy{654874}` |
| Bit-O-Asm-3 | 2619997 | `academy{2619997}` |
| Bit-O-Asm-4 | 654773 | `academy{654773}` |

## Useful Assembly Notes

- `mov`: copies a value between registers, memory, or an immediate value.
- `DWORD PTR`: specifies a 4-byte memory operand.
- `[rbp-offset]`: refers to a memory location at an offset below the base pointer.
- `imul`: signed integer multiplication.
- `add` / `sub`: addition / subtraction.
- `cmp`: compares operands by setting CPU flags; it does not store a result.
- `jle`: jumps if the comparison satisfies signed less-than-or-equal.
- `jmp`: unconditional jump.
- `eax`: the lower 32 bits of `rax`; these challenges ask for its value in decimal.
