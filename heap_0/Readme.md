# heap 0

## Challenge Information

- **Category:** Binary Exploitation
- **Difficulty:** Easy
- **Challenge:** Heap 0
- **Platform:** CYLab Academy
- **Target:** `xebec.cylabacademy.net:<TARGET-PORT>`
- **Vulnerability:** Heap-Based Buffer Overflow

## Objective

Exploit a heap-based buffer overflow to modify the value of `safe_var` and satisfy the program's win condition to retrieve the flag.

## Tools Used

- Kali Linux
- Netcat (`nc`)
- GCC (optional, for local compilation and testing)
- Linux terminal

## Source Code Analysis

The program allocates two buffers on the heap:

```c
#define INPUT_DATA_SIZE 5
#define SAFE_VAR_SIZE 5

input_data = malloc(INPUT_DATA_SIZE);
strncpy(input_data, "pico", INPUT_DATA_SIZE);

safe_var = malloc(SAFE_VAR_SIZE);
strncpy(safe_var, "bico", SAFE_VAR_SIZE);
```

The first allocation stores the user-controlled input, while the second stores `safe_var`, initially containing `"bico"`.

### Vulnerable Function

```c
void write_buffer() {
    printf("Data for buffer: ");
    fflush(stdout);
    scanf("%s", input_data);
}
```

The use of `scanf("%s", input_data)` without a field-width limit allows input longer than the allocated buffer. This can overwrite adjacent heap data.

### Win Condition

```c
void check_win() {
    if (strcmp(safe_var, "bico") != 0) {
        printf("\nYOU WIN\n");
        // Print flag
    }
}
```

The program prints the flag when `safe_var` no longer matches `"bico"`. Therefore, overwriting this variable is sufficient; code execution or a return-address overwrite is unnecessary.

## Exploitation Steps

### 1. Connect to the Challenge

Connect to the remote challenge instance using Netcat:

```bash
nc xebec.cylabacademy.net <TARGET-PORT>
```

The program displays the initial heap state and a menu of available operations.

### 2. Identify the Heap Addresses

The challenge displayed:

```text
[*]   0x64cf06c1f2b0  ->   pico
[*]   0x64cf06c1f2d0  ->   bico
```

The addresses correspond to `input_data` and `safe_var`, respectively.

Calculate their difference:

```text
0x64cf06c1f2d0 - 0x64cf06c1f2b0 = 0x20
```

Therefore, the two pointers are **32 bytes apart** in this challenge instance.

### 3. Send the Overflow Payload

Choose menu option `2` to write to the input buffer.

Enter the following payload containing 36 `A` characters:

```text
AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
```

The payload extends beyond the first allocation and overwrites the contents of `safe_var`.

The offset is determined from the displayed heap addresses rather than from the requested allocation size alone.

### 4. Trigger the Win Condition

After submitting the payload, choose menu option `4` to invoke `check_win()`.

The comparison against `"bico"` fails, and the server prints the flag.

## Flag

```text
academy{my_first_heap_overflow_2802614f}
```

## Technical Explanation

The vulnerability occurs because the program does not enforce the size of the input buffer. Although `input_data` was allocated only 5 bytes, `scanf("%s", input_data)` can write a much longer string.

The heap allocator placed the two allocations 32 bytes apart in this instance. The 36-byte payload extended into the second allocation and changed `safe_var`.

Since the program checks only whether `safe_var` differs from `"bico"`, the corrupted value satisfies the win condition.

## Lessons Learned

- Heap memory is also vulnerable to buffer overflows.
- `malloc()` allocates memory but does not automatically enforce bounds on subsequent writes.
- Unbounded string input can corrupt adjacent heap allocations.
- Heap addresses and allocation spacing can be inspected to determine an overflow offset.
- A successful binary exploitation challenge does not always require shellcode or control-flow hijacking.

## Mitigation

Use bounded input operations and validate input length against the destination buffer's actual capacity. For example:

```c
if (fgets(input_data, INPUT_DATA_SIZE, stdin) == NULL) {
    // Handle input failure
}
```

This limits the number of bytes read into the buffer, including the terminating null byte.

For production code, also consider safer allocation and string-handling patterns, compiler hardening, and runtime memory-safety tools.

## Conclusion

Heap 0 demonstrates how an unchecked write can corrupt a neighboring heap allocation. By identifying the distance between `input_data` and `safe_var`, sending a controlled overflow, and triggering the program's comparison, the challenge's win condition was satisfied and the flag was recovered.