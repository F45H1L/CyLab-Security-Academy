n0s4n1ty 1 — Exploitation Plan
Link: 
Category: Web Exploitation
Primary vulnerability: Unrestricted/unsanitized file upload
Objective: Obtain access to the challenge machine and retrieve the flag from /root.

1. Objective

The challenge describes a profile-picture upload feature whose implementation is flawed.

The intended attack path appears to be:

Web Application
      │
      ▼
Profile Picture Upload
      │
      ▼
Insufficient File Validation
      │
      ▼
Upload Executable File
      │
      ▼
Obtain Remote Shell
      │
      ▼
sudo -l
      │
      ▼
Privilege Escalation
      │
      ▼
/root/flag

The two explicit clues strongly suggest:

The upload mechanism may allow a file containing executable server-side code.
Once a shell is obtained, sudo -l may reveal a command that can be abused for privilege escalation.
2. Reconnaissance

First, understand the application rather than immediately attacking it.

Tasks
Identify the target URL/IP and port.
Open the website normally.
Locate the profile-picture upload functionality.
Identify:
Upload endpoint
HTTP method
Form parameter name
Accepted file extensions
Content-Type handling
Where uploaded files are stored
Whether uploaded files are directly accessible
Useful observations

When uploading a legitimate image, inspect the request in Burp Suite:

POST /<upload-endpoint>
Content-Type: multipart/form-data

Record the relevant parameter, for example:

file=<uploaded file>

Don't assume the endpoint or parameter names until we observe them.

3. Test the File-Upload Validation

The first hypothesis is:

The server validates the uploaded file poorly or not at all.

We should test progressively rather than jumping straight to exploitation.

Test A — Normal image

Upload:

test.jpg

Determine:

Is the upload accepted?
What response is returned?
Where does the application say the file was stored?
Test B — Extension manipulation

Depending on the server-side technology, test whether executable extensions are accepted.

For example, if the application appears to be PHP-based:

test.php

If the application appears to use another server-side technology, adapt the test to that environment.

Test C — MIME-type manipulation

Observe whether the server trusts:

Content-Type: image/jpeg

instead of checking the actual contents.

For example, an uploaded file may have a misleading filename/content type.

Test D — Filename handling

Check whether the application performs validation based solely on:

filename

or:

extension

rather than inspecting the file contents.

4. Determine Whether Uploaded Files Are Executed

This is the critical branch.

Finding that an arbitrary file can be uploaded does not automatically mean code execution.

We need to establish:

Can upload arbitrary file?
        │
        ├── Yes
        │
        ▼
Is uploaded file web-accessible?
        │
        ├── Yes
        │
        ▼
Does the server interpret it as executable code?
        │
        ├── Yes → Potential RCE
        │
        └── No → Investigate other upload paths

If the challenge's stack supports server-side scripting, a minimal proof-of-execution payload can be used only against the CTF target.

The goal at this stage is simply to demonstrate:

attacker-controlled request
          ↓
server executes attacker-controlled code

rather than immediately attempting complicated exploitation.

5. Shell Acquisition

If server-side code execution is confirmed, move from simple command execution toward a shell.

Potential approaches depend on the environment:

command execution through the uploaded file
a controlled reverse shell
an interactive shell if the challenge infrastructure permits it

Before doing this, establish:

Target IP
Attacker IP
Attacker listening port
Target operating system
Web-server user

Once a shell is obtained, stabilize it if necessary.

6. Initial Host Enumeration

Immediately establish who we are and where we are.

Useful commands:

whoami
id
hostname
pwd
uname -a

Then inspect the filesystem relevant to the challenge:

ls -la

The objective is not yet to search the entire filesystem blindly.

7. Follow the Challenge Hint: sudo -l

This should be one of the first privilege checks.

Run:

sudo -l

Record:

Which commands the current user can execute with sudo
Whether a password is required
Whether NOPASSWD is present
Whether wildcards or unrestricted arguments are permitted
Whether the permitted binary has known privilege-escalation possibilities

Expected conceptual path:

Low-privileged web user
        │
        ▼
sudo -l
        │
        ▼
Misconfigured sudo permission
        │
        ▼
Root shell / root command execution

We should not assume what the sudo entry will be until we see it.

8. Privilege Escalation

After obtaining the sudo -l output, identify the exact permitted binary.

Then determine whether that binary can:

execute another program
spawn a shell
read/write arbitrary files
execute commands through an option
invoke an interpreter
load a configuration/plugin
modify files executed by root

The escalation should be based on the actual sudo configuration, not a generic exploit.

9. Flag Retrieval

Once root privileges are obtained:

whoami

Expected:

root

Then inspect the target directory:

ls -la /root

Locate the challenge flag and read it.

The final objective is:

/root/<flag>
Decision Tree
START
  │
  ▼
Open challenge website
  │
  ▼
Find profile upload
  │
  ▼
Capture upload request
  │
  ▼
Understand validation
  │
  ├── Strong validation ──► Investigate bypasses
  │
  └── Weak/no validation
            │
            ▼
      Upload controlled file
            │
            ▼
      Determine storage path
            │
            ▼
      Determine execution
            │
       ┌────┴────┐
       │         │
      No        Yes
       │         │
       ▼         ▼
  Investigate   Command
  other flaws   execution
                   │
                   ▼
               Shell
                   │
                   ▼
                whoami
                   │
                   ▼
                sudo -l
                   │
                   ▼
          Identify sudo weakness
                   │
                   ▼
            Privilege escalation
                   │
                   ▼
                 root
                   │
                   ▼
             /root/flag
                   │
                   ▼
                  END
Tools

We'll likely use:

Browser — application behavior
Burp Suite — intercept/modify upload requests
Nmap — service identification if needed
curl — reproduce HTTP requests
Linux shell — post-exploitation enumeration
Evidence to record

For each step, keep:

Target:
Open ports:
Web technology:
Upload endpoint:
Upload parameter:
Accepted extension:
Uploaded-file location:
File execution confirmed:
Initial shell user:
sudo -l output:
Privilege-escalation vector:
Flag location:

Important: We should work only against the CTF machine provided for the challenge. Once you give me the target URL/IP, we can proceed through this plan step-by-step, starting with reconnaissance rather than jumping directly to exploitation.