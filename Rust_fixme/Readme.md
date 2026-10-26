# Rust Fixme

## Overview

This repository documents the solutions to three Rust programming challenges:

1. **Rust Fixme 1** — Basic Rust syntax errors
2. **Rust Fixme 2** — Mutable references and borrowing
3. **Rust Fixme 3** — Unsafe Rust and raw pointers

The objective of each challenge is to correct the provided Rust source code, compile it using Cargo, and execute the program to retrieve the flag.

## Prerequisites

- Kali Linux or another Linux distribution
- Rust toolchain (`rustc` and `cargo`)
- Internet connection to download dependencies
- Basic familiarity with the Linux terminal

Check whether Rust and Cargo are installed:

```bash
rustc --version
cargo --version
```

If they are not installed, install Rust using the official Rust installation instructions at https://www.rust-lang.org/tools/install.

---

## Rust Fixme 1

### Objective

Fix basic syntax errors in the Rust program so that it can decrypt and print the flag.

### Step 1: Download and extract

```bash
cd ~/Downloads

wget "https://challenge-files.cylabacademy.net/library/b9cfc84c442958b8ac848ddc0692bed9ef01a15877b8d37f36afbe68b5849acc/fixme1.tar.gz" -O fixme1.tar.gz

tar -xzf fixme1.tar.gz
cd fixme1
```

### Step 2: Inspect the source code

```bash
cat -n src/main.rs
```

The source contains three intentional errors.

### Step 3: Fix the syntax errors

**Error 1: Missing semicolon**

Original:

```rust
let key = String::from("CSUCKS")
```

Corrected:

```rust
let key = String::from("CSUCKS");
```

Rust statements generally end with a semicolon when they are not being used as the final expression of a block.

**Error 2: Incorrect return statement**

Original:

```rust
ret;
```

Corrected:

```rust
return;
```

The `return` keyword exits the current function.

**Error 3: Incorrect `println!` formatting**

Original:

```rust
println!(
    ":?",
    String::from_utf8_lossy(&decrypted_buffer)
);
```

Corrected:

```rust
println!(
    "{}",
    String::from_utf8_lossy(&decrypted_buffer)
);
```

The `{}` placeholder tells the `println!` macro to display the supplied value.

### Step 4: Compile and execute

```bash
cargo check
cargo run
```

### Result

Flag:

```text
academy{4r3_y0u_4_ru$t4c30n_n0w?}
```

### What I learned

- Rust statement syntax and semicolons
- Returning from functions using `return`
- Printing values with the `println!` macro
- Using Cargo to compile and run Rust projects

---

## Rust Fixme 2

### Objective

Correct the program's borrowing and mutability errors so that the `decrypt` function can modify a borrowed string.

### Step 1: Download and extract

```bash
cd ~/Downloads

wget "https://challenge-files.cylabacademy.net/library/048c73621a97b98cde18eae5a9495279f2396cb64c65235bb310e7ad26d726ac/fixme2.tar.gz" -O fixme2.tar.gz

tar -xzf fixme2.tar.gz
cd fixme2
```

### Step 2: Inspect the source code

```bash
cat -n src/main.rs
```

The function receives a string reference and attempts to append text to it. However, the reference and original variable are immutable.

### Step 3: Fix the borrowing errors

**Error 1: The function accepts an immutable reference**

Original:

```rust
fn decrypt(encrypted_buffer: Vec<u8>, borrowed_string: &String) {
```

Corrected:

```rust
fn decrypt(encrypted_buffer: Vec<u8>, borrowed_string: &mut String) {
```

A mutable reference allows the function to modify the borrowed string.

**Error 2: The original string variable is immutable**

Original:

```rust
let party_foul = String::from("Using memory unsafe languages is a: ");
```

Corrected:

```rust
let mut party_foul = String::from("Using memory unsafe languages is a: ");
```

The `mut` keyword allows the variable to be mutably borrowed.

**Error 3: The function call passes an immutable reference**

Original:

```rust
decrypt(encrypted_buffer, &party_foul);
```

Corrected:

```rust
decrypt(encrypted_buffer, &mut party_foul);
```

The `&mut` operator passes a mutable reference to the function.

### Step 4: Compile and execute

```bash
cargo check
cargo run
```

### Result

Flag:

```text
academy{4r3_y0u_h4v1n5_fun_y31?}
```

### What I learned

- The difference between `&String` and `&mut String`
- Declaring mutable variables with `let mut`
- Passing mutable references to functions
- How Rust's borrowing rules control modification of data

---

## Rust Fixme 3

### Objective

Restore the missing `unsafe` block around an operation that creates a slice from a raw pointer.

### Step 1: Download and extract

```bash
cd ~/Downloads

wget "https://challenge-files.cylabacademy.net/library/6df8b7779faf29e53f515922041d99bc26381a332ca090c291ba20577ac7cf92/fixme3.tar.gz" -O fixme3.tar.gz

tar -xzf fixme3.tar.gz
cd fixme3
```

### Step 2: Inspect the source code

```bash
cat -n src/main.rs
```

The source contains a commented-out `unsafe` block surrounding the decryption and raw-pointer operations.

### Step 3: Restore the `unsafe` block

The original code has the opening and closing braces commented out:

```rust
// unsafe {
    let decrypted_buffer = xrc.decrypt_vec(encrypted_buffer);

    let decrypted_ptr = decrypted_buffer.as_ptr();
    let decrypted_len = decrypted_buffer.len();

    let decrypted_slice =
        std::slice::from_raw_parts(decrypted_ptr, decrypted_len);

    borrowed_string.push_str(&String::from_utf8_lossy(decrypted_slice));
// }
```

Uncomment the block delimiters:

```rust
unsafe {
    let decrypted_buffer = xrc.decrypt_vec(encrypted_buffer);

    let decrypted_ptr = decrypted_buffer.as_ptr();
    let decrypted_len = decrypted_buffer.len();

    let decrypted_slice =
        std::slice::from_raw_parts(decrypted_ptr, decrypted_len);

    borrowed_string.push_str(&String::from_utf8_lossy(decrypted_slice));
}
```

Keep the rest of the source code unchanged.

### Why this works

The function `std::slice::from_raw_parts()` is unsafe because Rust cannot verify all of the memory-safety requirements associated with the raw pointer.

An `unsafe` block allows the operation to be performed, but it does not automatically make the operation safe. The programmer must ensure that:

- The pointer is valid and properly aligned.
- The pointer refers to enough initialized elements for the specified length.
- The referenced memory remains valid for the lifetime of the resulting slice.
- The memory is not modified in a way that violates Rust's aliasing requirements while the slice is in use.

In this challenge, the decrypted buffer remains alive while the slice is used.

### Step 4: Compile and execute

```bash
cargo check
cargo run
```

### Result

Flag:

```text
academy{n0w_y0uv3_f1x3d_1h3m_411}
```

### What I learned

- The purpose of `unsafe` blocks in Rust
- Raw pointers and slices
- The requirements of `std::slice::from_raw_parts()`
- Why unsafe operations require additional care from the programmer

---

## Summary of Flags

| Challenge | Main concept | Flag |
|---|---|---|
| Rust Fixme 1 | Basic syntax and formatting | `academy{4r3_y0u_4_ru$t4c30n_n0w?}` |
| Rust Fixme 2 | Mutable borrowing | `academy{4r3_y0u_h4v1n5_fun_y31?}` |
| Rust Fixme 3 | Unsafe Rust and raw pointers | `academy{n0w_y0uv3_f1x3d_1h3m_411}` |

## General Lessons Learned

These challenges demonstrate how Rust's compiler helps developers identify mistakes and enforce language rules.

The key takeaways are:

1. **Read compiler errors carefully.** They often point directly to the incorrect line and suggest a correction.
2. **Understand mutability.** A variable must be declared mutable before it can be mutably borrowed.
3. **Understand references.** Use `&mut` when a function needs to modify borrowed data.
4. **Use Cargo.** `cargo check` helps identify compilation problems, while `cargo run` builds and executes the program.
5. **Treat unsafe code carefully.** An `unsafe` block permits certain operations but does not remove the programmer's responsibility for memory safety.
6. **Read comments and hints.** In beginner CTF challenges, comments often identify the intended learning objective.

## Conclusion

All three Rust Fixme challenges were completed by examining the source code, correcting the intended errors, and running the programs with Cargo.

The challenges provided practical experience with Rust syntax, mutable borrowing, and unsafe operations.