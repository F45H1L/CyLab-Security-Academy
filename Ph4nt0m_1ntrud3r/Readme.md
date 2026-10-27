# Ph4nt0m 1ntrud3r

## Challenge Information

- **Challenge Name:** Ph4nt0m 1ntrud3r
- **Category:** Digital Forensics
- **Difficulty:** Easy
- **Objective:** Analyze the provided PCAP file, identify the concealed data, reconstruct the encoded fragments, and recover the flag.

## Challenge Description

A digital intruder has stolen sensitive information from a system. The objective is to investigate the network traffic in the supplied PCAP file and determine how the data was concealed and transmitted.

### Hints

1. Filter packets to narrow down the search.
2. The attack was performed in a specific time sequence.
3. Time is essential.

## Tools Used

- **Wireshark:** Graphical network traffic analysis.
- **TShark:** Command-line packet analysis.
- **xxd:** Hexadecimal-to-binary conversion.
- **Base64:** Decoding the concealed data.
- **sort:** Ordering extracted fragments by timestamp.

## Step 1: Download the PCAP File

Download the challenge capture from the following link:

[Download myNetworkTraffic.pcap](https://challenge-files.cylabacademy.net/library/828ca80eed550dbc9645ad6c2aa5c2061b2dd2e8e4098b1c7fe6d639459ea5a5/myNetworkTraffic.pcap)

Open a terminal in the directory containing the downloaded file.

Inspect the capture metadata:

```bash
capinfos myNetworkTraffic.pcap
```

This command displays information such as the capture duration, packet count, and file type.

## Step 2: Filter TCP Packets

The first step is to identify packets containing TCP application data.

Run:

```bash
tshark -r myNetworkTraffic.pcap \
  -Y 'tcp.len > 0' \
  -T fields \
  -e frame.number \
  -e frame.time_epoch \
  -e tcp.payload
```

### Explanation

- `-r myNetworkTraffic.pcap` reads the capture file.
- `-Y 'tcp.len > 0'` filters packets containing TCP payload data.
- `-T fields` outputs selected fields.
- `-e frame.number` displays the packet number.
- `-e frame.time_epoch` displays the packet timestamp.
- `-e tcp.payload` displays the TCP payload in hexadecimal.

The output contains hexadecimal payloads that can be converted into readable text.

## Step 3: Convert Hexadecimal Payloads

Each TCP payload is represented as hexadecimal bytes. These bytes can be converted into their original text representation using `xxd`.

For example, the following payload:

```text
59574e685a47567465513d3d
```

can be decoded with:

```bash
echo '59574e685a47567465513d3d' | xxd -r -p
```

Output:

```text
YWNhZGVteQ==
```

The result is a Base64-encoded string rather than the final plaintext.

Decode it using:

```bash
echo 'YWNhZGVteQ==' | base64 -d
```

Output:

```text
academy
```

This reveals the flag prefix.

## Step 4: Decode the Remaining Fragments

Further investigation reveals additional Base64-encoded fragments in the TCP payloads.

The following commands decode the fragments identified during the analysis.

### Fragment 1

```bash
echo 'ezF0X3c0cw==' | base64 -d
```

Output:

```text
{1t_w4s
```

### Fragment 2

```bash
echo 'bnRfdGg0dA==' | base64 -d
```

Output:

```text
nt_th4t
```

### Fragment 3

```bash
echo 'XzM0c3lfdA==' | base64 -d
```

Output:

```text
_34sy_t
```

### Fragment 4

```bash
echo 'YmhfNHJfNg==' | base64 -d
```

Output:

```text
bh_4r_6
```

### Fragment 5

```bash
echo 'YjA5ODYxZg==' | base64 -d
```

Output:

```text
b09861f
```

### Final Fragment

```bash
echo 'fQ==' | base64 -d
```

Output:

```text
}
```

## Step 5: Reconstruct the Flag

The fragments must be assembled in the correct order. The packet timestamps provide the ordering information needed to reconstruct the original message.

The decoded fragments are:

| Order | Decoded content |
|---|---|
| 1 | `academy` |
| 2 | `{1t_w4s` |
| 3 | `nt_th4t` |
| 4 | `_34sy_t` |
| 5 | `bh_4r_6` |
| 6 | `b09861f` |
| 7 | `}` |

Concatenating the fragments produces the final flag.

## Step 6: Verify the Reconstructed Data

The fragments can also be concatenated in their Base64 representation and decoded together.

```bash
echo 'YWNhZGVteQ==ezF0X3c0cw==bnRfdGg0dA==XzM0c3lfdA==YmhfNHJfNg==YjA5ODYxZg==fQ==' \
  | base64 -d
```

Expected output:

```text
academy{1t_w4snt_th4t_34sy_tbh_4r_6b09861f}
```

This confirms the reconstructed flag.

## Final Flag

```text
academy{1t_w4snt_th4t_34sy_tbh_4r_6b09861f}
```

## Attack Analysis

The capture demonstrates a form of concealed data transmission using encoded TCP payloads.

The data was split into multiple fragments, with each fragment encoded using Base64. The hexadecimal representation of the payloads added another layer that initially obscured the readable content.

The investigation involved:

1. Filtering TCP packets that contained payload data.
2. Extracting packet timestamps and hexadecimal payloads.
3. Converting hexadecimal bytes into Base64 text.
4. Decoding the Base64 fragments.
5. Ordering and concatenating the fragments to reconstruct the flag.

The challenge's timing hint is important because sorting the packets by capture order alone does not necessarily reflect the intended message order.

## Lessons Learned

- **Packet filtering:** Filtering traffic reduces the amount of data that must be examined.
- **Layered encoding:** Hexadecimal and Base64 representations can conceal readable information without providing encryption.
- **Timestamp analysis:** Packet timestamps can reveal the intended sequence of transmitted fragments.
- **Payload inspection:** Application data can contain information that is not visible from packet headers alone.
- **Data reconstruction:** Fragmented data must be correctly ordered before it can be decoded into the original message.

In a real investigation, timestamps should be considered alongside TCP sequence numbers and stream reassembly, since packet timing alone does not always establish the correct data order.

## Conclusion

The challenge was solved by analyzing the PCAP file, extracting TCP payloads, converting hexadecimal data into Base64 strings, and decoding the fragments in their intended chronological order.

The reconstructed flag confirms that the concealed message was successfully recovered from the network capture.