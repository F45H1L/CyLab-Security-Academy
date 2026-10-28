# Verify

## Challenge Information

- **Category:** Forensics
- **Difficulty:** Easy
- **Platform:** CYLAB Academy
- **Target:** `xebec.cylabacademy.net`
- **Port:** `<TARGET-PORT>`

## Objective

Identify the authentic file by comparing its SHA-256 checksum against the checksum provided by the challenge. Once verified, decrypt the file to retrieve the flag.

## Step 1: Connect to the Challenge

Connect to the remote server using SSH:

```bash
ssh -p <TARGET-PORT> ctf-player@xebec.cylabacademy.net
```

Enter the provided password when prompted and accept the SSH fingerprint by typing `yes` if requested.

## Step 2: Explore the Environment

Check the current user and list the available files:

```bash
whoami
pwd
ls
```

The challenge directory contains:

```text
checksum.txt
decrypt.sh
files/
```

The `files/` directory contains multiple candidate files. Only the file matching the trusted SHA-256 checksum should be decrypted.

## Step 3: Calculate and Verify SHA-256 Checksums

The challenge provides the following expected checksum:

```text
bf7701262e3fce45531b13d9866fb0913847a669823064aec1f60be7c2591ba8
```

Calculate the SHA-256 hash of every file in the directory:

```bash
sha256sum files/*
```

To find the file whose hash matches the expected checksum, filter the output using `grep`:

```bash
sha256sum files/* | grep 'bf7701262e3fce45531b13d9866fb0913847a669823064aec1f60be7c2591ba8'
```

The matching result was:

```text
bf7701262e3fce45531b13d9866fb0913847a669823064aec1f60be7c2591ba8  files/b6225255
```

The hash matches the expected checksum exactly, confirming that `files/b6225255` is the file to decrypt.

## Step 4: Decrypt the Verified File

Execute the provided decryption script against the verified file:

```bash
./decrypt.sh files/b6225255
```

The script successfully decrypts the file and prints the flag.

## Step 5: Retrieve the Flag

```text
academy{trust_but_verify_b6225255}
```

## Key Concepts Learned

- **SHA-256:** A cryptographic hash function that produces a 256-bit digest used to check data integrity.
- **File integrity verification:** Comparing a file's calculated hash against a trusted checksum to detect unexpected changes.
- **Command-line pipelines:** Using `|` to pass the output of `sha256sum` to `grep` for filtering.
- **Safe decryption workflow:** Verifying a file before processing it with the provided decryption script.

## Conclusion

The challenge demonstrated how cryptographic checksums can distinguish a file matching a trusted reference from other files. By calculating the SHA-256 hashes, identifying the matching file, and running the decryption script, the authentic flag was recovered.

**Lesson:** Trust, but verify.