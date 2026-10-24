# Corrupted File

## Challenge Information

- **Challenge Name:** Corrupted File
- **Category:** Forensics
- **Difficulty:** Easy
- **File Provided:** `file`
- **Objective:** Restore a corrupted JPEG image by repairing its file signature and ending marker.

## Challenge Description

The provided file appeared to be corrupted or unrecognized. The challenge hinted that modifying a few bytes in the file header could restore it.

The hints suggested:
1. Check the file header.
2. The file is a JPEG image.
3. Use tools such as `xxd` or `hexdump` to inspect and edit bytes.

## Step-by-Step Solution

### Step 1: Identify the File Type

First, navigate to the directory containing the downloaded file and use the `file` command:

```bash
file file
```

**Output:**

```text
file: data
```

The operating system could not identify the file as a recognized format. This suggested that the file's signature might have been corrupted.

### Step 2: Inspect and Edit the Hexadecimal Data

Open the file using `hexedit`:

```bash
hexedit file
```

Inspect the beginning and end of the file. JPEG images have characteristic marker bytes that help identify their format.

The original file had an invalid or corrupted signature. I modified the hexadecimal bytes so that the file started with:

```text
FF D8 FF D0
```

I also ensured that the file ended with:

```text
FF D9
```

**Note:** `FF D8` is the JPEG Start of Image (SOI) marker, and `FF D9` is the End of Image (EOI) marker. The `FF D0` byte sequence is a JPEG restart marker, not the usual JPEG/JFIF header sequence. The standard JFIF header typically begins with `FF D8 FF E0`, while other valid JPEG files may use different markers after SOI.

### Step 3: Verify the Repaired File

After editing the bytes, run the `file` command again:

```bash
file file
```

**Output:**

```text
file: JPEG image data, JFIF standard 1.01, aspect ratio,
density 1x1, segment length 16, baseline, precision 8,
800x500, components 3
```

The file was now recognized as a JPEG image with a resolution of 800 × 500 pixels.

### Step 4: Open the Recovered Image

Open the repaired file:

```bash
xdg-open file
```

The image opened successfully and revealed the challenge flag.

## Flag

```text
academy{r3st0r1ng_th3_by73s_d28873c8}
```

## Tools Used

- `file` — Identify the file format.
- `hexedit` — Inspect and modify hexadecimal bytes.
- `xdg-open` — Open the recovered image in the desktop environment.

Alternative tools for inspecting hexadecimal data include:

```bash
xxd file | head
xxd file | tail
hexdump -C file | head
hexdump -C file | tail
```

## Key Takeaways

- File extensions do not determine a file's actual format.
- File signatures, also called magic bytes, help identify file types.
- A corrupted header can prevent an operating system from recognizing an otherwise recoverable file.
- JPEG images use `FF D8` as the Start of Image marker and `FF D9` as the End of Image marker.
- After repairing a file, verify its format and open it to confirm that the content is recoverable.

## Conclusion

The challenge was solved by inspecting the file's hexadecimal representation, repairing the JPEG markers, and opening the recovered image. The flag was successfully extracted from the image.

**Challenge Status:** Solved