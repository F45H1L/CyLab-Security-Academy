# HashingJobApp – picoMini 2022

**Category:** General Skills  
**Difficulty:** Easy  
**Author:** LT 'syreal' Jones  
**Platform:** picoCTF  
**Tools:** Netcat (`nc`), MD5 (`md5sum`)

## Challenge Description

The challenge tests basic knowledge of MD5 hashing. The server provides text strings, and we must calculate their MD5 hashes and submit the correct values to receive the flag.

## Objective

Connect to the remote server, calculate the MD5 hash of each given string (excluding quotation marks), and submit the hashes.

## Solution

### Step 1: Connect to the server

Open the Kali Linux terminal and execute:

```bash
nc xebec.cylabacademy.net 47417
```

### Step 2: Calculate the MD5 hashes

The server provides three strings to hash.

**Question 1:**
```text
China
```

Generate the hash:

```bash
echo -n "China" | md5sum
```

Output:
```text
ae54a5c026f31ada088992587d92cb3a
```

**Question 2:**
```text
a used car lot
```

Generate the hash:

```bash
echo -n "a used car lot" | md5sum
```

Output:
```text
1a78bc064f8df8b4192ce5f8972c9b80
```

**Question 3:**
```text
bad dogs
```

Generate the hash:

```bash
echo -n "bad dogs" | md5sum
```

Output:
```text
60cc96ffdc458c98395d6e7b6878a6e9
```

### Step 3: Submit the hashes

Submit each generated MD5 hash in the active Netcat session.

The server responds with `Correct.` when the submitted hash matches the expected value.

After successfully answering all three questions, the server reveals the flag.

## Flag

```text
academy{4ppl1c4710n_r3c31v3d_8c3fe27c}
```

## Important Note

Use `echo -n` instead of `echo` when generating hashes.

The `-n` option prevents an additional newline character from being appended to the input. Without it, the generated MD5 hash will be different.

## Key Takeaways

- Understanding MD5 hashing.
- Generating hashes using Linux command-line utilities.
- Using Netcat to interact with remote challenge servers.
- Understanding how whitespace and newline characters affect hash values.

**Status:** Successfully Completed ✅