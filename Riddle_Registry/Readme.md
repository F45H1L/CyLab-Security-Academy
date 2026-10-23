# Riddle Registry

## Challenge Overview

- **Challenge Name:** Riddle Registry
- **Category:** Forensics
- **Difficulty:** Easy
- **Objective:** Recover a hidden flag from the metadata of a PDF document.

### Challenge Description

The challenge provides a PDF file named `confidential.pdf`. Its visible content appears to be garbled nonsense, but the hints indicate that the visible text is a decoy and that the flag is hidden in the document's metadata.

**Hints:**
1. Don't be fooled by the visible text; it's just a decoy!
2. Look beyond the surface for hidden clues.

## Tools Used

- `wget` — Download the challenge PDF.
- `ExifTool` — Inspect PDF metadata.
- `base64` — Decode the hidden flag.

## Step 1: Download the PDF

Download the document from the official challenge URL:

```bash
wget -O confidential.pdf "https://challenge-files.cylabacademy.net/library/b32f1f1945481591df61d328d02995fe3521d16b70928fbe44e3bd90c4ccb113/confidential.pdf"
```

Verify that the file was downloaded:

```bash
ls -lh confidential.pdf
```

The downloaded file is approximately 178 KiB.

## Step 2: Inspect the PDF Metadata

Use ExifTool to examine the document's metadata:

```bash
exiftool confidential.pdf
```

Among the output fields, the following `Author` value is particularly important:

```text
Author : YWNhZGVteXtwdXp6bDNkX20zdGFkYXRhX2YwdW5kIV85ZDNjYzY2OX0=
```

The value does not appear to be ordinary text. Its structure suggests that it may be encoded using Base64.

**Key observation:** The challenge does not require decoding the visible PDF content. The flag is stored in the metadata's `Author` field.

## Step 3: Decode the Author Field

Use the `base64` utility to decode the value:

```bash
echo 'YWNhZGVteXtwdXp6bDNkX20zdGFkYXRhX2YwdW5kIV85ZDNjYzY2OX0=' | base64 -d
```

The command produces the following output:

```text
academy{puzzl3d_m3tadata_f0und!_9d3cc669}
```

## Step 4: Verify the Result

The decoded value follows the CyLab Academy flag format:

```text
academy{...}
```

This confirms that the hidden metadata contains the challenge flag.

## Final Flag

```text
academy{puzzl3d_m3tadata_f0und!_9d3cc669}
```

## Technical Explanation

PDF documents can store metadata independently of their visible page content. This information may include fields such as:

- `Author`
- `Title`
- `Subject`
- `Keywords`
- `Creator`
- `Producer`

In this challenge, the `Author` metadata field contains a Base64-encoded string.

Base64 is an encoding scheme that represents binary data using printable characters. It is not encryption and does not require a secret key. Anyone who identifies the encoding can decode the value using standard tools.

The investigation therefore follows this sequence:

1. Download the PDF.
2. Inspect the document metadata.
3. Identify the suspicious `Author` field.
4. Recognize the Base64-encoded value.
5. Decode the value to recover the flag.

## Lessons Learned

- Always inspect file metadata during digital forensics investigations.
- Visible document content may be a decoy.
- ExifTool provides a quick way to examine metadata in PDF and other file formats.
- Base64-encoded strings can often be identified by their character set and padding.
- Encoding is not the same as encryption; Base64 does not provide confidentiality.

## Conclusion

The Riddle Registry challenge was solved by examining the metadata of `confidential.pdf`, identifying a Base64-encoded value in the `Author` field, and decoding it to recover the flag.

The challenge demonstrates the importance of examining both the visible content and the underlying metadata of files during forensic analysis.