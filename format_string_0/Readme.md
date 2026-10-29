# Format String 0

## Challenge Information

- **Challenge:** Format String 0
- **Category:** Binary Exploitation
- **Difficulty:** Easy
- **Target:** `xebec.cylabacademy.net:16888`
- **Vulnerability:** Format String Vulnerability

## Objective

Exploit format string vulnerabilities in the provided C program to satisfy two customers and retrieve the flag.

## Tools Required

- Kali Linux
- Netcat (`nc`)
- GCC (optional, for local analysis)
- A text editor to inspect the provided C source

## Step 1: Analyze the Source Code

The program provides two customers with predefined burger menus. It validates each choice using `strcmp()` before passing the selected input directly to `printf()`.

The first vulnerable statement is:

```c
int count = printf(choice1);

if (count > 2 * BUFSIZE) {
    serve_bob();
}
```

The buffer size is defined as:

```c
#define BUFSIZE 32
```

Therefore, the program proceeds to the second customer when `printf()` outputs more than 64 characters.

The second vulnerable statement is:

```c
printf(choice2);
```

Both statements use user-controlled input as the format string instead of using a fixed format string such as `printf("%s", choice1);`.

## Step 2: Connect to the Challenge

Connect to the remote instance using Netcat:

```bash
nc xebec.cylabacademy.net 16888
```

The server displays Patrick's burger menu and asks for a recommendation.

## Step 3: Exploit Patrick's Format String

Enter the following burger choice:

```text
Gr%114d_Cheese
```

This is a valid menu item, so it passes the menu validation.

The `%114d` format specifier instructs `printf()` to print an integer in a field at least 114 characters wide. Since the program does not provide a corresponding integer argument, the behavior is undefined, but the field-width requirement ensures that the formatted output exceeds 64 characters on the observed target.

The program therefore satisfies the condition:

```c
count > 2 * BUFSIZE
```

It then calls `serve_bob()` and displays SpongeBob's menu.

## Step 4: Exploit SpongeBob's Format String

At the second prompt, enter:

```text
Cla%sic_Che%s%steak
```

This is also a valid menu item.

The program executes:

```c
printf(choice2);
```

The three `%s` specifiers instruct `printf()` to read string pointers from arguments that were never supplied. This invokes undefined behavior and may cause invalid memory access.

In the observed remote session, the output contained duplicated text and `(null)`, followed by the flag. The challenge is designed to use a segmentation fault handler to print the flag when `SIGSEGV` occurs.

## Step 5: Retrieve the Flag

The program registers the following signal handler:

```c
void sigsegv_handler(int sig) {
    printf("\n%s\n", flag);
    fflush(stdout);
    exit(1);
}
```

When the intended segmentation-fault path is triggered, the handler prints the flag stored in `flag`.

**Flag:**

```text
academy{7h3_cu570m3r_15_n3v3r_SEGFAULT_f8db0cb4}
```

## Complete Exploit Inputs

Run the following command:

```bash
nc xebec.cylabacademy.net 16888
```

Enter these inputs sequentially at the prompts:

1. Patrick:

   ```text
   Gr%114d_Cheese
   ```

2. SpongeBob:

   ```text
   Cla%sic_Che%s%steak
   ```

The flag is printed after the second input triggers the challenge's intended flag-printing path.

## Vulnerability Analysis

### Root Cause

The program uses input strings directly as `printf()` format strings. This allows the user to control how the function interprets format specifiers.

### Impact

Depending on the format specifiers, arguments, memory layout, and runtime, format string vulnerabilities can lead to:

- Unexpected memory reads
- Information disclosure
- Crashes caused by invalid memory accesses
- Memory writes when dangerous specifiers such as `%n` are used under suitable conditions

### Recommended Fix

Replace:

```c
printf(choice1);
printf(choice2);
```

With:

```c
printf("%s", choice1);
printf("%s", choice2);
```

Using a fixed format string ensures that user-provided percent signs are treated as ordinary string content rather than format directives.

The program should also validate input lengths, use bounded input functions, and avoid relying on crashes as part of application logic.

## Key Takeaways

- Format string vulnerabilities occur when user-controlled data is passed directly as the format argument to functions such as `printf()`.
- `%114d` controls the minimum width of a formatted integer field.
- `%s` dereferences a string pointer supplied through a variadic argument.
- Calling `printf()` with missing format arguments causes undefined behavior.
- Signal handlers can change how a program responds to certain runtime errors, but a segmentation fault should never be relied upon as normal application logic.

## Conclusion

The challenge was solved by exploiting two format string vulnerabilities. The first input generated enough formatted output to reach SpongeBob's menu. The second input triggered the intended failure path, allowing the program's signal handler to reveal the flag.