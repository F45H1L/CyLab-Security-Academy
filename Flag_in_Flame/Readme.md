# Flag in Flame

**Category:** Forensics  
**Difficulty:** Easy  
**Platform:** CyLab Academy

## Challenge Description

The SOC team discovered a suspiciously large log file containing encoded text instead of typical log entries. The objective was to decode the data, reconstruct the hidden image, and retrieve the flag.

## Tools Used

- Kali Linux
- `wget`
- `base64`
- `file`
- Image viewer

## Solution

### Step 1: Download the File

Downloaded the encoded log data using `wget`.

```bash
wget -O logs.txt "https://challenge-files.cylabacademy.net/library/0246cbed3fe2edb81de38ddfb60f948020c30448c0c21fcd82cee316fb9da111/logs.txt"
```

### Step 2: Decode the Base64 Data

The challenge hint suggested Base64 decoding. Decoded the contents and saved the output as an image file.

```bash
base64 -d logs.txt > logs.png
```

### Step 3: Identify and Open the Image

Verified the resulting file type:

```bash
file logs.png
```

Opened the decoded image and examined its contents to locate the hidden flag.

### Step 4: Retrieve the Flag

Recovered the flag from the decoded image.

## Flag

```text
academy{forensics_analysis_is_amazing_8dbd3133}
```

## What I Learned

- How to recognize Base64-encoded data.
- How to reconstruct an image from encoded text using Linux command-line tools.
- How to use file identification and image inspection during digital forensics investigations.

## Conclusion

Successfully completed the Flag in Flame challenge by decoding Base64 data into an image and extracting the hidden flag.