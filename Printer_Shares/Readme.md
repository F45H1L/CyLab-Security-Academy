# Printer Shares

## Challenge Description

> Oops! Someone accidentally sent an important file to a network printer—can you retrieve it from the print server?

**Target:**

```text
chatelaine.cylabacademy.net:<LAB-ID>
```

### Hints

* Knowing how the **SMB protocol** works would be helpful.
* `smbclient` and `smbutil` are useful tools.

---

## 1. Check the Port

First, verify that the target is reachable on port `<LAB-ID>`.

```bash
nc -vz chatelaine.cylabacademy.net <LAB-ID>
```

Expected output:

```text
Connection to chatelaine.cylabacademy.net (<LAB-IP>) <LAB-ID> port [tcp/*] succeeded!
```

This confirms that the service is accessible.

---

## 2. Enumerate SMB Shares

Since the challenge hints at SMB, use `smbclient` to enumerate the available shares.

```bash
smbclient -L //chatelaine.cylabacademy.net -p <LAB-ID> -N
```

The `-L` option lists available shares, while `-p <LAB-ID>` specifies the custom SMB port. The `-N` option attempts the connection without a password.

The server returned:

```text
Sharename       Type      Comment
---------       ----      -------
shares          Disk      Public Share With Guests
IPC$            IPC       IPC Service (Samba 4.19.5-Ubuntu)
```

The important share is:

```text
shares
```

> Note: Although the challenge describes a printer/print server, the exposed SMB share is named `shares`.

---

## 3. Connect to the SMB Share

Connect to the discovered `shares` share using anonymous/guest access:

```bash
smbclient //chatelaine.cylabacademy.net/shares -p <LAB-ID> -N
```

If successful, an SMB prompt appears:

```text
smb: \>
```

---

## 4. List the Files

Use `ls` to enumerate the contents of the share:

```text
ls
```

The server returned:

```text
.                                   D        0
..                                  D        0
dummy.txt                           N     1142
flag.txt                            N       37
```

The `flag.txt` file is the interesting file.

---

## 5. Download the Flag

From the SMB prompt, retrieve the file:

```text
get flag.txt
```

Output:

```text
getting file \flag.txt of size 37 as flag.txt
```

Exit the SMB session:

```text
exit
```

---

## 6. Read the Flag

The downloaded file can now be read locally:

```bash
cat flag.txt
```

Flag:

```text
academy{5mb_pr1nter_5h4re5_c549fc78}
```

---

## Solution Summary

The solution was:

```text
Port <LAB-ID>
    ↓
SMB service
    ↓
Enumerate shares with smbclient
    ↓
Discover "shares"
    ↓
Connect anonymously
    ↓
List files
    ↓
Find flag.txt
    ↓
Download with get
    ↓
Read flag
```

### Commands Used

```bash
nc -vz chatelaine.cylabacademy.net <LAB-ID>

smbclient -L //chatelaine.cylabacademy.net -p <LAB-ID> -N

smbclient //chatelaine.cylabacademy.net/shares -p <LAB-ID> -N

# Inside smbclient:
ls
get flag.txt
exit

cat flag.txt
```

## Flag

```text
academy{5mb_pr1nter_5h4re5_c549fc78}
```

## Skills Learned

* SMB share enumeration
* Using `smbclient`
* Connecting to SMB on a non-standard port
* Anonymous/guest SMB access
* Listing files in an SMB share
* Downloading files from an SMB server