# Scan Surprise — CTF Write-up

## Challenge Information

- **Challenge Name:** Scan Surprise
- **Category:** Forensics
- **Difficulty:** Easy
- **Platform:** CyLab Academy
- **Objective:** Decode a QR code image to retrieve the flag.

## Challenge Description

The challenge provides a QR code image named `flag.png`. Instead of displaying the flag as plain text, the challenge stores the flag inside the QR code.

The objective is to analyze the image, decode the QR code, and retrieve the hidden flag.

## Tools Used

- SSH — to access the remote challenge instance.
- `zbarimg` — to decode QR codes from image files.

## Step 1: Connect to the Challenge

Connect to the remote instance using SSH:

```bash
ssh -p <TARGET-PORT> ctf-player@chatelaine.cylabacademy.net
```

Enter the provided password when prompted:

```text
13792798
```

Accept the SSH fingerprint by entering `yes` if prompted.

The challenge displays the QR code upon connecting. The image file is available in the `drop-in` directory.

## Step 2: Locate the Image

List the files in the current directory:

```bash
ls
```

The directory contains:

```text
flag.png
```

## Step 3: Decode the QR Code

Use `zbarimg` to scan the image:

```bash
zbarimg flag.png
```

The tool successfully decodes the QR code and displays the following output:

```text
QR-Code:academy{p33k_@_b00_ebb7f3f7}
scanned 1 barcode symbols from 1 images in 0 seconds
```

The decoded text contains the challenge flag.

Alternatively, suppress additional output using:

```bash
zbarimg --quiet flag.png
```

## Step 4: Retrieve the Flag

**Flag:**

```text
academy{p33k_@_b00_ebb7f3f7}
```

## Understanding the D-Bus Warning

During execution, the tool may display an error similar to:

```text
Connection Error (Failed to connect to socket /run/dbus/system_bus_socket: No such file or directory)
Connection Null
```

This warning indicates that the D-Bus system service is unavailable in the challenge environment. It does not prevent the QR code from being decoded.

The successful scan confirmation shows that the image was processed correctly.

## How It Works

1. A QR code encodes information in a two-dimensional visual pattern.
2. The `zbarimg` utility analyzes the image and detects the QR code.
3. The utility decodes the pattern into its original text representation.
4. The decoded text reveals the flag.

## Conclusion

The challenge demonstrates a basic digital forensics technique: extracting embedded information from an image using barcode-analysis tools.

The flag was successfully recovered by decoding `flag.png` with `zbarimg`, without requiring manual image analysis or a mobile phone.

**Status:** Solved