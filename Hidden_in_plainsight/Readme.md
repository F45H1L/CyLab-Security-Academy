# Hidden in Plainsight

**Category:** Forensics  
**Difficulty:** Easy  
**Challenge:** Hidden in Plainsight

## Challenge Description

You're given a seemingly ordinary JPG image. The objective is to investigate the image, discover hidden information, and recover the flag.

## Tools Used

- `ExifTool` — Inspect image metadata
- `Base64` — Decode the hidden metadata value
- `Steghide` — Extract hidden data from the JPEG
- `cat` — Read the extracted flag

## Step 1: Inspect the Image Metadata

Use ExifTool to examine the image's metadata.

```bash
exiftool img.jpg
```

The output contains a suspicious `Comment` field:

```text
Comment : c3RlZ2hpZGU6Y0VGNmVuZHZjbVE9
```

This encoded string suggests that the metadata contains a hidden clue.

## Step 2: Decode the Metadata

Decode the comment using Base64:

```bash
echo 'c3RlZ2hpZGU6Y0VGNmVuZHZjbVE9' | base64 -d
```

Output:

```text
steghide:cEF6endvcmQ=
```

The decoded text identifies **Steghide** as the tool to use and provides another Base64-encoded string.

Decode the second string:

```bash
echo 'cEF6endvcmQ=' | base64 -d
```

Output:

```text
pAzzword
```

This reveals the passphrase required to extract the hidden data.

## Step 3: Extract the Hidden File

Use Steghide to extract the embedded data from the JPEG:

```bash
steghide extract -sf img.jpg
```

When prompted, enter the passphrase:

```text
pAzzword
```

Steghide successfully extracts the hidden file:

```text
wrote extracted data to "flag.txt".
```

## Step 4: Retrieve the Flag

Read the contents of the extracted file:

```bash
cat flag.txt
```

The output contains the challenge flag.

## Flag

```text
academy{h1dd3n_1n_1m4g3_71ae1957}
```

## What I Learned

- Image metadata can contain clues that are not visible when viewing an image.
- Base64 encoding can conceal useful information such as tool names and passphrases.
- Steghide can extract data embedded within supported image files when the correct passphrase is supplied.
- A systematic forensic workflow can reveal hidden content in an otherwise ordinary-looking JPEG.

## Conclusion

The challenge was solved by inspecting the JPEG metadata, decoding the Base64-encoded comment twice, identifying Steghide and its passphrase, and extracting the hidden flag from the image.