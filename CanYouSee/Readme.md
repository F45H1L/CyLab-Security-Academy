# CanYouSee

## Challenge Information
- **Challenge Name:** CanYouSee
- **Category:** Forensics
- **Difficulty:** Easy
- **Objective:** Recover the hidden flag from image metadata.

---

## 1. Download the Challenge File

Download the challenge ZIP archive and extract its contents.

```bash
unzip unknown.zip
```

The extracted file is:

```text
ukn_reality.
```

Rename the extracted file to `.jpg` if necessary.

```bash
mv ukn_reality. ukn_reality.jpg
```

---

## 2. Analyze Image Metadata Using ExifTool

Use `exiftool` to inspect the image metadata.

```bash
exiftool ukn_reality.jpg
```

The output contains several metadata fields, including an unusual **Attribution URL** field:

```text
Attribution URL : YWNhZGVteXtNRTc0RDQ3QV9ISUREM05fYWViZThjMGZ9Cg==
```

This value appears to be Base64-encoded data.

---

## 3. Decode the Hidden Metadata

Copy the suspicious metadata value and decode it using the Base64 utility.

```bash
echo 'YWNhZGVteXtNRTc0RDQ3QV9ISUREM05fYWViZThjMGZ9Cg==' | base64 -d
```

Decoded output:

```text
academy{ME74D47A_HIDD3N_aebe8c0f}
```

---

## 4. Flag

```text
academy{ME74D47A_HIDD3N_aebe8c0f}
```

---

## 5. Vulnerability / Technique Used

**Technique:** Metadata Analysis + Base64 Decoding

The challenge hides the flag inside an image's metadata rather than its visible content.

The `Attribution URL` metadata field contains a Base64-encoded string. By extracting and decoding this value, the hidden flag can be recovered.

### Tools Used
- `unzip` – Extract the challenge archive.
- `ExifTool` – Inspect image metadata.
- `base64` – Decode the hidden metadata value.

---

## 6. Key Takeaways

- Images may contain hidden information in metadata fields.
- Metadata should be inspected during digital forensic investigations.
- Suspicious metadata values may contain encoded data.
- Base64 is an encoding scheme, not encryption.
- ExifTool is useful for discovering hidden information in image files.

**Challenge successfully solved!**