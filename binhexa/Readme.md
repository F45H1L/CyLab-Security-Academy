# binhexa

**Category:** General Skills
**Difficulty:** Easy
**Challenge Name:** binhexa

## Objective

Perform six binary operations in the specified order using two given binary numbers. Submit the result of each operation in binary format and convert the final operation's result into hexadecimal to obtain the flag.

## Challenge Setup

Connect to the challenge server using Netcat:

```bash
nc xebec.cylabacademy.net <TARGET-IP>
```

The server provides two binary numbers:

```text
Binary Number 1: 01110110
Binary Number 2: 11100001
```

Convert both binary numbers into decimal for easier arithmetic:

[
01110110_2=118\_{10}
]

[
11100001_2=225\_{10}
]

## 1. Right Shift (>>)

**Operation:** Shift Binary Number 2 right by 1 bit.

### Formula

[
N >> k = \left\lfloor\frac{N}{2^k}\right\rfloor
]

A right shift by one bit is equivalent to integer division by 2.

### Calculation

```text
Binary Number 2: 11100001

Right shift by 1:

11100001 >> 1 = 01110000
```

**Result:**

```text
01110000
```

## 2. Bitwise OR (|)

**Operation:** Perform OR on Binary Number 1 and Binary Number 2.

### Formula

[
A|B
]

OR produces `1` if at least one corresponding bit is `1`.

| A | B | A OR B |
| - | - | ------ |
| 0 | 0 | 0      |
| 0 | 1 | 1      |
| 1 | 0 | 1      |
| 1 | 1 | 1      |

### Calculation

```text
  01110110
| 11100001
-----------
  11110111
```

**Result:**

```text
11110111
```

## 3. Binary Multiplication (\*)

**Operation:** Multiply Binary Number 1 and Binary Number 2.

### Formula

[
A\times B
]

Binary multiplication follows the same rules as decimal multiplication:

[
0\times0=0
]

[
0\times1=0
]

[
1\times0=0
]

[
1\times1=1
]

### Calculation

Convert binary numbers to decimal:

[
A=118,\quad B=225
]

Multiply:

[
118\times225=26550
]

Convert decimal 26550 into binary:

[
26550\_{10}=110011110110110_2
]

**Result:**

```text
110011110110110
```

## 4. Bitwise AND (&)

**Operation:** Perform AND on Binary Number 1 and Binary Number 2.

### Formula

[
A&B
]

AND produces `1` only when both corresponding bits are `1`.

| A | B | A AND B |
| - | - | ------- |
| 0 | 0 | 0       |
| 0 | 1 | 0       |
| 1 | 0 | 0       |
| 1 | 1 | 1       |

### Calculation

```text
  01110110
& 11100001
-----------
  01100000
```

**Result:**

```text
01100000
```

## 5. Binary Addition (+)

**Operation:** Add Binary Number 1 and Binary Number 2.

### Formula

[
A+B
]

Binary addition rules:

| A | B | Sum | Carry |
| - | - | --- | ----- |
| 0 | 0 | 0   | 0     |
| 0 | 1 | 1   | 0     |
| 1 | 0 | 1   | 0     |
| 1 | 1 | 0   | 1     |

### Calculation

Convert to decimal:

[
118+225=343
]

Convert decimal 343 into binary:

[
343\_{10}=101010111_2
]

**Result:**

```text
101010111
```

## 6. Left Shift (<<)

**Operation:** Shift Binary Number 1 left by 1 bit.

### Formula

[
N<\<k=N\times2^k
]

A left shift by one bit is equivalent to multiplication by 2.

### Calculation

```text
Binary Number 1: 01110110

Left shift by 1:

01110110 << 1 = 11101100
```

**Result:**

```text
11101100
```

Decimal verification:

[
118\times2=236
]

[
236\_{10}=11101100_2
]

## Final Binary to Hexadecimal Conversion

The challenge asks for the final operation's result in hexadecimal.

Final binary result:

```text
11101100
```

### Formula

Group binary digits into sets of four, starting from the right:

```text
1110 1100
```

Convert each group into its hexadecimal equivalent:

| Binary | Hexadecimal |
| ------ | ----------- |
| 0000   | 0           |
| 0001   | 1           |
| 0010   | 2           |
| 0011   | 3           |
| 0100   | 4           |
| 0101   | 5           |
| 0110   | 6           |
| 0111   | 7           |
| 1000   | 8           |
| 1001   | 9           |
| 1010   | A           |
| 1011   | B           |
| 1100   | C           |
| 1101   | D           |
| 1110   | E           |
| 1111   | F           |

Therefore:

```text
1110 = E
1100 = C
```

Final hexadecimal result:

```text
EC
```

Submit `EC` to the challenge server.

## Flag

```text
academy{b1tw^3se_0p3eR@tI0n_su33essFuL_e453a847}
```

## Key Concepts Learned

- Binary number representation
- Binary to decimal conversion
- Binary to hexadecimal conversion
- Bitwise OR (`|`)
- Bitwise AND (`&`)
- Binary addition (`+`)
- Binary multiplication (`*`)
- Right shift (`>>`)
- Left shift (`<<`)
- Powers of two and shift operations

## Conclusion

The challenge demonstrated how basic binary arithmetic and bitwise operations work. By correctly applying all six operations in sequence and converting the final binary result into hexadecimal, the flag was successfully obtained.

**Status:** Completed
**Flag Obtained:** Yes