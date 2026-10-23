# CyLab Academy — Binary Digits

## Challenge Information

- **Challenge Name:** Binary Digits
- **Category:** Forensics
- **Difficulty:** Easy
- **Platform:** CyLab Academy

## Challenge Description

The challenge provides a file named `digits.bin` containing a sequence of `0`s and `1`s. Although the file initially appears to contain meaningless data, the binary digits encode meaningful information.

The objective is to decode the binary data, reconstruct the original file, and recover the hidden flag.

## Tools Used

- Kali Linux
- `wget` — Download the challenge file
- Python 3 — Convert binary digits into bytes
- `file` — Identify the recovered file type
- `xxd` — Inspect the file signature
- Image viewer — View the recovered image

## Solution

### Step 1: Download the Challenge File

Download the provided file using `wget`:

```bash
wget "https://challenge-files.cylabacademy.net/library/819e05da202722f7847eab5c0efdedc194be8a2bc19e6b436d057c8514ae70bc/digits.bin" -O digits.bin
```

### Step 2: Inspect the File

Check the file type and inspect its contents:

```bash
file digits.bin
head -c 100 digits.bin
wc -c digits.bin
```

The file contains binary digits represented as ASCII characters. These characters must be converted into actual binary bytes to recover the encoded data.

### Step 3: Decode the Binary Data

Each group of eight binary digits represents one byte. Convert each group from base 2 into an integer between 0 and 255, then write the resulting bytes to a new file.

Run the following Python script:

```python
with open("digits.bin", "r") as f:
    data = "".join(f.read().split())

if not data or any(bit not in "01" for bit in data):
    raise ValueError("Input contains unexpected characters")

if len(data) % 8 != 0:
    raise ValueError("Bit count is not divisible by 8")

decoded = bytes(
    int(data[i:i + 8], 2)
    for i in range(0, len(data), 8)
)

with open("decoded.jpg", "wb") as f:
    f.write(decoded)

print(f"Decoded {len(decoded)} bytes")
print("Recovered file: decoded.jpg")
```

Save the script as `decode.py` and execute it:

```bash
python3 decode.py
```

The script removes whitespace, validates the binary digits, converts every eight bits into a byte, and saves the reconstructed data as `decoded.jpg`.

### Step 4: Verify the Recovered File

Identify the file type:

```bash
file decoded.jpg
```

Inspect the first 16 bytes:

```bash
xxd -l 16 decoded.jpg
```

A JPEG image commonly starts with the hexadecimal signature `ff d8 ff`. This signature helps confirm whether the recovered data is a JPEG image.

### Step 5: Open the Image

Open the reconstructed image:

```bash
xdg-open decoded.jpg
```

Inspect the image to locate the hidden flag.

## Flag

```text
academy{h1dd3n_1n_th3_b1n4ry_5a7375c5}
```

## Technical Explanation

The original file represents binary data using the ASCII characters `0` and `1`. These characters are not themselves the encoded image bytes; they are a textual representation of the bits.

The decoding process works as follows:

1. Read the binary digits from the file.
2. Remove whitespace and validate the input.
3. Divide the bit sequence into groups of eight.
4. Convert each group from binary to its corresponding byte value.
5. Write the resulting bytes into a new file.
6. Verify the recovered file signature.
7. Open the image and inspect it for the flag.

For example:

```text
01001000
```

Converting this binary value to decimal gives `72`, which corresponds to the ASCII character `H`.

When the same process is applied to the complete bit sequence, the bytes reconstruct the encoded image.

## Key Takeaways

- Files that appear to contain meaningless binary digits may encode images or other file formats.
- Eight bits form one byte.
- Python's `int(value, 2)` converts a binary string into an integer.
- Python's `bytes()` allows the reconstructed byte sequence to be written as a binary file.
- File signatures, also known as magic bytes, help identify file formats.
- File extensions alone do not prove a file's actual format; inspecting its contents is important.

## Conclusion

The Binary Digits challenge demonstrates a fundamental digital forensics technique: converting a textual representation of binary data into raw bytes to reconstruct an embedded file. By decoding the bit sequence, verifying the resulting file format, and inspecting the recovered image, the hidden flag was successfully recovered.