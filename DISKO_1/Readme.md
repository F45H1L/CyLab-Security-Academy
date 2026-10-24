# DISKO 1

**Category:** Digital Forensics  
**Difficulty:** Easy

## Challenge Description

Can you find the flag in this disk image?

**Challenge File:** `disko-1.dd.gz`

## Tools Used

- **Kali Linux**
- `gunzip` — to decompress the downloaded file
- `strings` — to extract readable text from the disk image
- `grep` — to search for the flag pattern

## Solution

### Step 1: Extract the Disk Image

The downloaded challenge file was compressed using gzip. First, extract the disk image:

```bash
gunzip disko-1.dd.gz
```

This produces the file `disko-1.dd`.

Alternatively, preserve the original compressed file using:

```bash
gzip -dk disko-1.dd.gz
```

### Step 2: Search for the Flag

Use the `strings` command to extract readable text from the disk image and pipe the output to `grep` to search for the expected flag format:

```bash
strings disko-1.dd | grep "academy{"
```

### Step 3: Retrieve the Flag

The command returns the following output:

```text
academy{1t5_ju5t_4_5tr1n9_3f5388a2}
```

## Flag

```text
academy{1t5_ju5t_4_5tr1n9_3f5388a2}
```

## Technical Explanation

A disk image is a raw representation of storage data. It can contain binary data alongside readable text.

- **`strings`** extracts sequences of printable characters from a binary file.
- **`grep "academy{"`** filters the extracted strings to display text containing the expected flag prefix.

The flag was stored as readable text within the disk image, so it could be recovered without mounting the image or performing complex forensic analysis.

## Key Takeaways

- Understand how to decompress `.gz` files in Linux.
- Use `strings` to inspect binary files for readable content.
- Combine Linux commands using pipes to filter investigation results.
- Start with simple forensic techniques before moving to more complex analysis.

## Conclusion

The challenge was solved by extracting the disk image and searching its readable strings for the CyLab Academy flag format. This demonstrates how basic command-line utilities can be effective for simple digital forensics challenges.