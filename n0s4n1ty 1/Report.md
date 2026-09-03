    # Web Application Security Test Report

## n0s4n1ty 1 — picoCTF

**Assessment Type:** Web Application Penetration Test
**Target:** `standard-pizzas.picoctf.net:50288`
**Environment:** Authorized CTF/Lab Environment
**Tester:** Security Researcher
**Assessment Status:** Completed
**Severity:** **Critical**

---

## 1. Executive Summary

A security assessment was performed against the `n0s4n1ty 1` web application. The assessment identified a critical **unrestricted file upload vulnerability** in the profile-picture functionality.

The application accepted a PHP file through the profile-picture upload mechanism and subsequently allowed the uploaded PHP file to be executed through the web server. This resulted in arbitrary operating-system command execution in the context of the `www-data` user.

Further enumeration revealed a critical privilege-management misconfiguration:

```text
User www-data may run the following commands on challenge:
    (ALL) NOPASSWD: ALL
```

This configuration allowed the compromised web-server account to execute arbitrary commands with root privileges without requiring authentication.

The combination of these vulnerabilities resulted in complete system compromise and allowed access to the protected `/root` directory.

The challenge flag was successfully retrieved from:

```text
/root/flag.txt
```

---

## 2. Scope

### Target

```text
http://standard-pizzas.picoctf.net:50288/
```

### Tested Functionality

* Web application
* Profile-picture upload functionality
* Uploaded-file execution
* Server-side command execution
* Local privilege escalation
* Root filesystem access

Testing was conducted solely against the authorized picoCTF challenge environment.

---

## 3. Methodology

The assessment followed a structured penetration-testing methodology:

1. Web application reconnaissance
2. Identification of file-upload functionality
3. Analysis of the upload mechanism
4. Testing of file-type validation
5. Verification of server-side PHP execution
6. Verification of operating-system command execution
7. Local privilege enumeration
8. Analysis of `sudo` configuration
9. Privilege escalation
10. Verification of root access
11. Retrieval of the challenge flag

---

# 4. Findings

## Finding 1 — Unrestricted File Upload Leading to Remote Code Execution

**Severity:** Critical
**Category:** Web Application / File Upload
**Impact:** Remote Code Execution

### Description

The application provides a profile-picture upload functionality that submits files to:

```text
upload.php
```

using the parameter:

```text
fileToUpload
```

The HTML implementation did not contain meaningful restrictions preventing server-side executable files from being uploaded.

The application subsequently accepted a PHP file and made it accessible through the web server.

The uploaded PHP file was executed successfully, demonstrating server-side code execution.

### Evidence

A PHP proof-of-concept was uploaded containing:

```php
<?php
echo "PHP_EXECUTION_CONFIRMED";
?>
```

Accessing the uploaded file returned:

```text
PHP_EXECUTION_CONFIRMED
```

This confirmed that the server was interpreting attacker-controlled uploaded content as PHP.

### Command Execution

The vulnerability was then used to execute operating-system commands through PHP.

For example:

```php
<?php
system("whoami");
?>
```

This demonstrated command execution in the context of the web-server account.

### Impact

An attacker capable of uploading a malicious PHP file can potentially:

* Execute arbitrary commands
* Read application files
* Access sensitive server resources
* Establish a persistent foothold
* Pivot to other systems
* Escalate privileges if local security controls are misconfigured

In this challenge, the vulnerability directly enabled the next stage of compromise.

---

## Finding 2 — Critical `sudo` Misconfiguration

**Severity:** Critical
**Category:** Privilege Escalation
**Impact:** Full Root Access

### Description

After obtaining command execution as `www-data`, local privilege enumeration was performed using:

```bash
sudo -l
```

The server returned:

```text
User www-data may run the following commands on challenge:
    (ALL) NOPASSWD: ALL
```

This configuration grants the `www-data` account unrestricted `sudo` access to execute commands as any user, including `root`, without requiring a password.

### Impact

An attacker who obtains command execution as `www-data` can immediately escalate to root privileges.

This transforms the initial web vulnerability from limited application-level compromise into **complete host compromise**.

### Root Access Verification

Root-level command execution was demonstrated using `sudo`.

The `/root` directory was subsequently enumerated:

```text
total 12
drwx------ 1 root root  22 Aug 21  2025 .
drwxr-xr-x 1 root root  39 Sep  3 10:24 ..
-rw-r--r-- 1 root root 571 Apr 10  2021 .bashrc
-rw-r--r-- 1 root root 161 Jul  9  2019 .profile
-rw-r--r-- 1 root root  36 Aug 21  2025 flag.txt
```

The presence of `flag.txt` confirmed access to the root user's directory.

---

# 5. Attack Chain

The complete attack path was:

```text
Profile Picture Upload
        │
        ▼
Insufficient File Validation
        │
        ▼
Upload PHP File
        │
        ▼
PHP Execution
        │
        ▼
Operating-System Command Execution
        │
        ▼
www-data
        │
        ▼
sudo -l
        │
        ▼
NOPASSWD: ALL
        │
        ▼
Root Privileges
        │
        ▼
/root/flag.txt
```

This demonstrates how two individually serious weaknesses combined to produce complete system compromise.

---

# 6. Proof of Compromise

The following conditions were successfully demonstrated:

| Test                              | Result     |
| --------------------------------- | ---------- |
| Profile-picture upload identified | PASS       |
| PHP file accepted                 | PASS       |
| Uploaded PHP file executed        | PASS       |
| OS command execution achieved     | PASS       |
| Execution context identified      | `www-data` |
| `sudo -l` accessible              | PASS       |
| `NOPASSWD: ALL` identified        | PASS       |
| Root command execution achieved   | PASS       |
| `/root` accessed                  | PASS       |
| Flag retrieved                    | PASS       |

---

# 7. Flag

The challenge flag was successfully retrieved from:

```text
/root/flag.txt
```

Flag:

```text
picoCTF{wh47_c4n_u_d0_wPHP_7189176f}
```

---

# 8. Risk Assessment

### Overall Risk: Critical

The combination of unrestricted file upload and unrestricted `sudo` privileges provides an attacker with a straightforward path from an unauthenticated web interaction to complete operating-system compromise.

The effective attack path requires no legitimate user password once the upload vulnerability is reached.

### Risk progression

```text
Unauthenticated Web Access
          ↓
Malicious File Upload
          ↓
Remote Code Execution
          ↓
Web Server Account
          ↓
Unrestricted sudo
          ↓
ROOT
```

---

# 9. Recommendations

## 9.1 Secure File Uploads

The application should:

* Allow only explicitly required file types.
* Validate file extensions using an allowlist.
* Validate the actual file contents rather than trusting the filename or MIME type.
* Reject server-side executable extensions.
* Rename uploaded files to randomly generated filenames.
* Store uploads outside the web root whenever possible.
* Prevent uploaded files from being interpreted as executable server-side code.
* Apply appropriate filesystem permissions to uploaded files and directories.

For profile pictures, the application should ideally process the image and generate a new safe image rather than storing arbitrary user-controlled content.

---

## 9.2 Correct `sudo` Configuration

The following configuration should be removed:

```text
(ALL) NOPASSWD: ALL
```

The `www-data` account should not have unrestricted `sudo` privileges.

If a web application genuinely requires a privileged operation, the sudo policy should:

* Permit only the specific required executable.
* Restrict arguments where possible.
* Avoid `NOPASSWD` unless strictly necessary.
* Never grant unrestricted root command execution to a web-server account.

---

## 9.3 Apply Least Privilege

The web-server account should operate with the minimum privileges necessary.

A compromise of `www-data` should not provide an attacker with a direct path to root.

Recommended controls include:

* Dedicated service accounts
* Minimal filesystem permissions
* Restricted sudo policies
* Process isolation
* Containerization where appropriate
* Mandatory access controls such as AppArmor or SELinux

---

# 10. Conclusion

The `n0s4n1ty 1` application was successfully compromised through an unrestricted file-upload vulnerability.

The ability to upload and execute a PHP file provided remote command execution as `www-data`. Subsequent enumeration revealed that `www-data` had unrestricted passwordless `sudo` privileges.

This allowed privilege escalation to root and access to `/root/flag.txt`.

The primary security lessons demonstrated by this challenge are:

1. **Never trust uploaded files.**
2. **Uploaded content must never be allowed to execute as server-side code.**
3. **Web-server accounts should not have unrestricted sudo privileges.**
4. **Least privilege is essential for limiting the impact of web application compromise.**
5. **A seemingly simple file-upload feature can become a full system compromise when combined with poor privilege management.**

**Assessment Result: CRITICAL — Full Host Compromise Demonstrated.**