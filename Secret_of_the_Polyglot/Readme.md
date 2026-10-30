# Secret of the Polyglot


## Challenge Description

The Network Operations Center (NOC) discovered a suspicious file that provided conflicting information about its actual file type.

The objective was to investigate the file, identify its multiple formats, extract the hidden information, and recover the flag.

**Challenge File:** `flag2of2-final.pdf`

---

## 1. Initial File Inspection

First, navigate to the Downloads directory and verify that the challenge file is available.

```bash
cd ~/Downloads
ls
```

## 2. Identify the Actual File Format

Although the file had a `.pdf` extension, we used `exiftool` to inspect its metadata.

```bash
exiftool flag2of2-final.pdf
```

### Important Output

```text
File Type          : PNG
File Type Extension: png
MIME Type          : image/png
Image Width        : 200
Image Height       : 40
Software           : GPL Ghostscript 10.02.1
Warning            : [minor] Trailer data after PNG IEND chunk
```

### Observation

The file was identified as a PNG image rather than a PDF.

The important clue was:

```text
Warning: Trailer data after PNG IEND chunk
```

A PNG file normally ends with an `IEND` chunk. The warning indicated that additional data existed after the PNG image data.

This suggested that the file contained multiple file formats.

---

## 3. Extract the PDF Content

Next, we attempted to read the file as a PDF using `pdftotext`.

```bash
pdftotext flag2of2-final.pdf output.txt
```

Read the extracted text:

```bash
cat output.txt
```

### Extracted Content

```text
1n_pn9_&_pdf_07226ac4}
```

This revealed one portion of the flag.

The file was therefore successfully interpreted as both a PNG image and a PDF document.

---

## 4. Analyze the File Structure Using Binwalk

To identify the different file signatures and their offsets, we used `binwalk`.

```bash
binwalk flag2of2-final.pdf
```

### Output

```text
DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
0             0x0             PNG image, 200 x 40, 4-bit colormap, non-interlaced
453           0x1C5           PDF document, version: "1.4"
688           0x2B0           Zlib compressed data, default compression
```

### Observation

The output identified three components:

| Offset | File Format | Description |
|---|---|---|
| 0 | PNG | Image containing the first flag portion |
| 453 | PDF 1.4 | Document containing the second flag portion |
| 688 | Zlib | Additional compressed data |

This confirmed that the suspicious file was a polyglot file.

---

## 5. Extract the PNG Image

Since the PNG data started at offset `0` and the PDF started at offset `453`, we extracted the first 453 bytes.

```bash
dd if=flag2of2-final.pdf of=hidden.png bs=1 count=453 status=none
```

Open the extracted image:

```bash
xdg-open hidden.png
```

### Extracted Flag Portion

```text
academy{f1u3n7_
```

This revealed the first portion of the flag.

---

## 6. Extract the PDF Document

The PDF document started at offset `453`.

We extracted the PDF portion:

```bash
dd if=flag2of2-final.pdf of=hidden.pdf bs=1 skip=453 status=none
```

Extract its text:

```bash
pdftotext hidden.pdf -
```

### Extracted Flag Portion

```text
1n_pn9_&_pdf_07226ac4}
```

---

## 7. Investigate the Zlib Data

Binwalk also identified Zlib compressed data at offset `688`.

We extracted it for further examination:

```bash
dd if=flag2of2-final.pdf of=compressed.zlib bs=1 skip=688 status=none
```

Attempt decompression using Python:

```bash
python3 -c "import zlib; d=open('compressed.zlib','rb').read(); print(zlib.decompress(d))"
```

The Zlib signature indicated another embedded data structure worth investigating, although the two flag portions had already been recovered.

---

## 8. Reconstruct the Flag

We obtained two portions of the flag:

**PNG portion:**

```text
academy{f1u3n7_
```

**PDF portion:**

```text
1n_pn9_&_pdf_07226ac4}
```

Combining both portions gives the complete flag.

## Flag

```text
academy{f1u3n7_1n_pn9_&_pdf_07226ac4}
```

---

## 9. Tools Used

| Tool | Purpose |
|---|---|
| ExifTool | Identify actual file format and inspect metadata |
| pdftotext | Extract text from the PDF layer |
| Binwalk | Identify embedded file formats and offsets |
| dd | Extract individual file components using byte offsets |
| xdg-open | Open the extracted PNG image |
| Python (zlib) | Investigate compressed data |

---

## 10. Key Takeaways

- A file extension does not necessarily represent its actual file format.
- File signatures (magic bytes) are useful for identifying suspicious files.
- Polyglot files can be interpreted as multiple file formats.
- Data can exist after a PNG's `IEND` chunk.
- Tools such as ExifTool, Binwalk, and `dd` are useful for file-format analysis and data extraction.

**Challenge Status:** Solved