# Blame Game

**Platform:** CyLab Academy  
**Category:** General Skills  
**Difficulty:** Easy  
**Technique:** Git History Analysis

## Challenge Description

Someone's commits seem to be preventing the program from working. The objective is to identify the person responsible by investigating the Git commit history.

### Hints

1. In collaborative projects, many users can make many changes. How can you see the changes within one file?
2. Read the Git chapter from picoPrimer.
3. You can use Python to execute the file, but execution is unnecessary.

---

## 1. Download the Challenge

Download the challenge ZIP file:

```bash
wget "https://challenge-files.cylabacademy.net/library/73faf59d9b8f1e6e13a9df9bd19d5e465e231ee0dcf06aaa8696640c437cefb5/challenge.zip" -O challenge.zip
```

Extract the archive:

```bash
unzip challenge.zip
```

Navigate into the extracted directory:

```bash
cd drop-in
```

List the contents:

```bash
ls -la
```

---

## 2. Identify the Git Repository

Check whether the directory contains Git metadata:

```bash
ls -la
```

The `.git` directory indicates that the challenge contains a Git repository.

Inspect the commit history:

```bash
git log
```

This displays the commits made by different contributors.

---

## 3. Investigate the File History

The challenge hint suggests examining the changes made to a particular file.

Use:

```bash
git log -- message.py
```

### Output

```text
commit 4d1cc14d74a0ac5fbb850760ad1e6171e26e8d5b
Author: academy{@sk_th3_1nt3rn_b568fb28} <ops@cylabacademy.org>
Date:   Wed Sep 23 05:06:57 2026 +0000

    optimize file size of prod code

commit d82ea2472532cb75fc54a1038409816ec31eb66e
Author: academy <ops@cylabacademy.org>
Date:   Wed Sep 23 05:06:57 2026 +0000

    create top secret project
```

The first commit contains an unusual author name.

---

## 4. Extract the Flag

The flag is embedded inside the Git commit author's name.

### Flag

```text
academy{@sk_th3_1nt3rn_b568fb28}
```

---

## 5. Understanding the Git Commands

### View Commit History

```bash
git log
```

Displays the commit history of the repository.

### View File-Specific Commit History

```bash
git log -- message.py
```

Displays commits that affected `message.py`.

### View Commit Details

```bash
git show 4d1cc14d74a0ac5fbb850760ad1e6171e26e8d5b
```

Displays the commit metadata and changes introduced by that commit.

### Identify Line Authors

```bash
git blame message.py
```

Shows who last modified each line of a file.

---

## 6. Key Concepts Learned

- Git version control
- Commit history enumeration
- File-specific history investigation
- Commit author identification
- Git metadata analysis
- Using Git to investigate suspicious changes

## Conclusion

The challenge was solved by inspecting the Git commit history of `message.py`.

The suspicious commit contained the flag in its author field, demonstrating how Git metadata can reveal information that is not directly visible in the program's source code.