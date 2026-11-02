# Commitment Issues

**Platform:** CyLab Academy  
**Category:** General Skills  
**Difficulty:** Easy  
**Technique:** Git History Enumeration & Secret Recovery

## Challenge Description

I accidentally wrote the flag down. Good thing I deleted it!

The challenge provides a ZIP archive containing a Git repository. The flag was removed from the current version of a file, but it remains accessible in the repository's commit history.

## Objective

Recover the deleted flag using Git version control history.

## Tools Used

- Git
- Linux Terminal
- unzip

## Solution

### Step 1: Extract the Challenge Files

Download and extract `challenge.zip`.

```bash
unzip challenge.zip
cd drop-in
```

### Step 2: Inspect the Repository

List the repository contents, including hidden files.

```bash
ls -la
```

The `.git` directory confirms that the extracted directory is a Git repository.

### Step 3: Enumerate Git Commit History

```bash
git log --oneline --all
```

Output:

```text
1e5983e (HEAD -> master) remove sensitive info
72b1219 create flag
```

The commit history reveals two commits:

- `72b1219` — create flag
- `1e5983e` — remove sensitive info

### Step 4: Inspect the Current File

```bash
cat message.txt
```

Output:

```text
TOP SECRET
```

The current version no longer contains the flag.

### Step 5: Inspect the Previous Commit

```bash
git show 72b1219
```

Output:

```diff
diff --git a/message.txt b/message.txt
new file mode 100644
index 0000000..a2e2a4d
--- /dev/null
+++ b/message.txt
@@ -0,0 +1 @@
+academy{s@n1t1z3_a001ab89}
```

The previous commit contains the original flag.

### Step 6: Recover the Flag Directly

To retrieve the previous version of `message.txt`:

```bash
git show 72b1219:message.txt
```

Recovered Flag:

```text
academy{s@n1t1z3_a001ab89}
```

## Flag

`academy{s@n1t1z3_a001ab89}`

## What I Learned

- Git maintains a history of committed changes.
- Removing sensitive information from a file does not remove it from previous commits.
- `git log` helps enumerate commit history.
- `git show` allows inspection of commits and historical file contents.
- `git show <commit>:<file>` retrieves a file from a specific commit without modifying the working directory.
- Sensitive information should never be committed to Git repositories, even temporarily.

## Vulnerability / Security Insight

**Exposed Secrets in Git History**

Sensitive information committed to a repository can remain accessible through historical commits even after it is deleted from the current version.

Deleting a secret from a file is not equivalent to removing it from Git history.

## References

- [Git Documentation](https://git-scm.com/docs)
- [CyLab Academy picoPrimer - Git Version Control](https://primer.cylabacademy.org/#_git_version_control)

---

**Challenge Completed Successfully!**