# repetitions

**Category:** General Skills
**Difficulty:** Easy

## Challenge Description

Can you make sense of this file?

**Hint:** Multiple decoding is always good.

## Solution

### Step 1: Download the file

```bash
wget -O enc_flag https://challenge-files.cylabacademy.net/library/ce0e4deafa3edf1cbf8c34435461a3eb00b7f26cad812b5dc8b774490b33a4c4/enc_flag
```

### Step 2: Decode the Base64 layers

The file contains multiple layers of Base64 encoding. Decode the contents repeatedly until the final flag appears.

```bash
cat enc_flag | base64 -d
```

Repeat the decoding process on each intermediate output.

Alternatively, use a Python script to automate the decoding:

```python
import base64

with open("enc_flag", "rb") as f:
    data = f.read().strip()

while True:
    try:
        decoded = base64.b64decode(data, validate=True)
        if decoded == data:
            break
        data = decoded
        print(data.decode(errors="replace"))
    except Exception:
        break
```

### Step 3: Retrieve the flag

After decoding all the Base64 layers, the final output is:

```text
academy{base64_n3st3d_dic0d!n8_d0wnl04d3d_0b83decb}
```

## Flag

```text
academy{base64_n3st3d_dic0d!n8_d0wnl04d3d_0b83decb}
```

## Key Takeaway

- Base64 is an encoding scheme, not encryption.
- Nested Base64 encoding can be decoded by repeatedly applying Base64 decoding.
- Recognizing patterns and automating repetitive decoding makes solving encoding challenges easier.