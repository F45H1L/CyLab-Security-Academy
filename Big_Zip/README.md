# Big Zip

**Category:** General Skills  
**Difficulty:** Easy

## Challenge Description

Unzip the provided archive and find the hidden flag.

## Solution

### Step 1: Download the ZIP File

Download the challenge archive from the provided link.

Alternatively, use `wget`:

```bash
wget "https://challenge-files.cylabacademy.net/library/b150f6f4d21f46fc2bba57ef49c7305541fa8e9bfa4241dfe8d066b99de56200/big-zip-files.zip"
```

### Step 2: Extract the Archive

Extract the ZIP file using:

```bash
unzip big-zip-files.zip
```

Move into the extracted directory:

```bash
cd big-zip-files
```

### Step 3: Search for the Flag

The archive contains many nested directories and files. Instead of manually opening each file, use recursive grep to search for the flag prefix.

```bash
grep -R "academy{" .
```

**Command breakdown:**

- `grep` – Searches for matching text.
- `-R` – Recursively searches through directories and subdirectories.
- `"academy{"` – The flag prefix to search for.
- `.` – Searches from the current directory.

### Step 4: Retrieve the Flag

The command returns the file path and matching text:

```text
./folder_pmbymkjcya/folder_cawigcwvgv/folder_ltdayfmktr/folder_fnpfclfyee/whzxrpivpqld.txt:information on the record will last a billion years. Genes and brains and books encode academy{gr3p_15_m4g1c_ef8790dc}
```

The flag is:

```text
academy{gr3p_15_m4g1c_ef8790dc}
```

## Final Flag

`academy{gr3p_15_m4g1c_ef8790dc}`

## What I Learned

- How to extract ZIP archives using `unzip`.
- How to navigate nested directories in Linux.
- How to use recursive searching with `grep -R`.
- How to locate hidden information efficiently without manually inspecting every file.

**Key Takeaway:** Recursive grep is extremely useful in CTF challenges involving large archives and deeply nested directory structures.