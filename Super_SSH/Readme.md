# Super SSH

## Challenge Information

- **Challenge Name:** Super SSH
- **Category:** General Skills
- **Difficulty:** Easy
- **Technique:** SSH (Secure Shell) Authentication
- **Username:** `ctf-player`
- **Hostname:** `chatelaine.cylabacademy.net`
- **Port:** `<TARGET-PORT>`

## Objective

Connect to the remote machine using SSH with the provided credentials and retrieve the challenge flag.

## Step 1: Establish SSH Connection

Open a terminal (Kali Linux or any system with an SSH client) and execute:

```bash
ssh -p <TARGET-PORT> ctf-player@chatelaine.cylabacademy.net
```

### Command Breakdown

- `ssh` – Initiates a Secure Shell connection.
- `-p <TARGET-PORT>` – Specifies the custom SSH port.
- `ctf-player` – Username used for authentication.
- `chatelaine.cylabacademy.net` – Remote server hostname.

## Step 2: Accept the SSH Fingerprint

During the first connection, SSH may display a host authenticity warning:

```text
Are you sure you want to continue connecting (yes/no/[fingerprint])?
```

Enter:

```text
yes
```

This accepts and stores the server's host key in the local `~/.ssh/known_hosts` file.

## Step 3: Authenticate with the Password

When prompted, enter the provided password:

```text
647e170e
```

**Note:** Password characters are not displayed while typing in the terminal. This is normal SSH behavior.

## Step 4: Retrieve the Flag

After successful authentication, the server displays the welcome message along with the challenge flag.

```text
Welcome ctf-player, here's your flag:
academy{s3cur3_c0nn3ct10n_a827223e}
```

## Flag

```text
academy{s3cur3_c0nn3ct10n_a827223e}
```

## What We Learned

1. How to establish a remote connection using SSH.
2. How to connect using a specific username.
3. How to specify a custom SSH port using the `-p` option.
4. How to accept an SSH host fingerprint.
5. How password-based SSH authentication works.

## Key Takeaway

The general SSH connection syntax is:

```bash
ssh -p <PORT> <USERNAME>@<HOSTNAME>
```

SSH provides an encrypted communication channel between a client and a remote server, allowing users to securely access remote systems.

**Challenge Status:** Solved