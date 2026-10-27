# RED

## Challenge Information

- **Challenge Name:** RED
- **Category:** Digital Forensics
- **Difficulty:** Easy
- **Platform:** CyLab Academy
- **File:** `red.png`

## Objective

Analyze the provided PNG image to discover hidden data and recover the challenge flag.

## Tools Used

- `exiftool` — to inspect image metadata
- `strings` — to extract readable strings from the PNG
- `zsteg` — to detect hidden data in PNG pixel channels
- `base64` — to decode the extracted payload

## Step 1: Download the Challenge File

Download `red.png` from the challenge's file link and navigate to the directory containing the image.

```bash
cd ~/Downloads
```

## Step 2: Inspect the Image Metadata

Run ExifTool to examine the image's metadata.

```bash
exiftool red.png
```

The output revealed an unusual metadata field named `Poem`:

```text
Poem : Crimson heart, vibrant and bold,
       Hearts flutter at your sight.
       Evenings glow softly red,
       Cherries burst with sweet life.
       Kisses linger with your warmth.
       Love deep as merlot.
       Scarlet leaves falling softly,
       Bold in every stroke.
```

The poem appears unrelated to the visible image, suggesting that it may contain a clue.

## Step 3: Identify the Hidden Hint

Take the first letter of each line of the poem:

| Line | First letter |
|---|---|
| Crimson heart, vibrant and bold | C |
| Hearts flutter at your sight | H |
| Evenings glow softly red | E |
| Cherries burst with sweet life | C |
| Kisses linger with your warmth | K |
| Love deep as merlot | L |
| Scarlet leaves falling softly | S |
| Bold in every stroke | B |

The letters spell:

```text
CHECKLSB
```

This indicates that the image should be investigated for **Least Significant Bit (LSB) steganography**.

## Step 4: Inspect Readable Strings

Run the `strings` command:

```bash
strings red.png
```

The output confirmed that the poem was embedded in the PNG metadata. However, the flag was not directly visible in the extracted strings, so pixel-level steganography analysis was necessary.

## Step 5: Install and Run zsteg

Install `zsteg` using RubyGems:

```bash
gem install zsteg
```

The installation succeeded, but the executable was not available through the default `PATH`.

Run it directly using its installation path:

```bash
~/.local/share/gem/ruby/3.3.0/bin/zsteg red.png
```

Alternatively, scan the image for a broader range of hidden-data patterns:

```bash
~/.local/share/gem/ruby/3.3.0/bin/zsteg -a red.png
```

## Step 6: Extract the Hidden Base64 Payload

The `zsteg` output revealed the following entry:

```text
b1,rgba,lsb,xy .. text:
"YWNhZGVteXtyM2RfMXNfdGgzX3VsdDFtNHQzX2N1cjNfZjByXzU0ZG4zNTVffQ==..."
```

The important fields are:

- `b1`: One bit per color channel.
- `rgba`: Red, Green, Blue, and Alpha channels.
- `lsb`: Least significant bit.
- `xy`: Pixels are traversed in XY order.

The output contained a repeated Base64 payload. One complete copy was sufficient to recover the flag.

## Step 7: Decode the Base64 String

Copy one complete Base64 payload from the `zsteg` output and decode it:

```bash
echo 'YWNhZGVteXtyM2RfMXNfdGgzX3VsdDFtNHQzX2N1cjNfZjByXzU0ZG4zNTVffQ==' | base64 -d
```

Decoded output:

```text
academy{r3d_1s_th3_ult1m4t3_cur3_f0r_54dn355_}
```

## Final Flag

```text
academy{r3d_1s_th3_ult1m4t3_cur3_f0r_54dn355_}
```

## Conclusion

The challenge demonstrated how hidden information can be embedded in an image without visibly changing its appearance.

The solution involved three key techniques:

1. **Metadata analysis:** The poem in the PNG metadata provided the initial clue.
2. **Acrostic analysis:** The first letters of the poem's lines spelled `CHECKLSB`.
3. **LSB steganography:** `zsteg` recovered a Base64-encoded payload from the RGBA channels, which was decoded to obtain the flag.

**Lesson learned:** In image-forensics challenges, inspect metadata before moving to steganography tools. Metadata may contain indirect clues that reveal the exact extraction technique required.