# Time Machine

## Challenge Information

- **Challenge Name:** Time Machine
- **Category:** General Skills
- **Difficulty:** Easy
- **Platform:** CYLab Academy
- **Key Concept:** Git Version Control
- **Tools Used:** Kali Linux, Git, Unzip

## Objective

Recover the information left behind in a Git commit history. The challenge provides a text file that hints at examining previous commits to discover what was being worked on.

## Step 1: Download and Extract the Challenge

Download the provided `challenge.zip` file and extract it into a directory named `time-machine`.

```bash
unzip challenge.zip -d time-machine
cd time-machine/drop-in
```

List all files, including hidden files and directories:

```bash
ls -la
```

**Observation:** The directory contains `message.txt` and a hidden `.git` directory. The `.git` directory indicates that this is a Git repository.

## Step 2: Read the Message

Read the contents of `message.txt`:

```bash
cat message.txt
```

Output:

```text
This is what I was working on, but I'd need to look at my commit history to know why...
```

**Analysis:** The message explicitly suggests inspecting the Git commit history rather than searching only the current file.

## Step 3: Inspect the Git Commit History

Run the following command:

```bash
git log --oneline --all
```

Output:

```text
c7dc026 (HEAD -> master) academy{t1m3m@ch1n3_b260a551}
```

**Observation:** The commit message contains the challenge flag.

## Step 4: Retrieve the Flag

The flag recovered from the Git commit history is:

```text
academy{t1m3m@ch1n3_b260a551}
```

## Final Flag

`academy{t1m3m@ch1n3_b260a551}`

## Technical Explanation

Git maintains a history of commits, and each commit includes metadata and a commit message. The `git log` command displays this history.

In this challenge, the flag was stored in the commit message instead of the contents of `message.txt`. Reading the file alone would not reveal the answer; examining the repository's commit history exposed the hidden information.

Useful Git commands:

| Command | Purpose |
|---|---|
| `ls -la` | Lists files and hidden directories |
| `cat message.txt` | Reads the message file |
| `git log --oneline --all` | Displays commit history in compact form |
| `git log --all --format=fuller` | Displays detailed commit metadata |
| `git show <commit-hash>` | Inspects a specific commit and its changes |

## Lessons Learned

- Hidden directories such as `.git` can contain important repository metadata.
- Git commit messages may contain information not present in the current working files.
- Always inspect commit history when a challenge hints at previous work or forgotten notes.
- `git log` is an essential command for Git-based CTF investigations.

## Conclusion

The challenge was solved by identifying the Git repository, reading the hint in `message.txt`, and inspecting the commit history using `git log --oneline --all`. The flag was recovered directly from the commit message without modifying the repository.