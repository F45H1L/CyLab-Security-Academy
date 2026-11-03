# Collaborative Development

## Challenge Description

My team has been working very hard on new features for our flag printing program! I wonder how they'll work together?

The objective is to integrate multiple Git feature branches into the main branch, resolve merge conflicts, and execute the flag printing program.


## 1. Inspect the Git Repository

Navigate to the extracted challenge directory:

```bash
cd ~/Downloads/drop-in
```

List the available branches:

```bash
git branch -a
```

Output:

```text
feature/part-1
feature/part-2
feature/part-3
main
```

Inspect the commit history:

```bash
git log --oneline --all --graph
```

This revealed three feature branches containing different parts of the flag printing program.

## 2. Configure Git

Configure Git identity to allow commits during merging:

```bash
git config user.name "Fashil"
git config user.email "fashil@example.com"
```

## 3. Merge Feature Branches

Switch to the main branch:

```bash
git checkout main
```

Merge the feature branches:

```bash
git merge feature/part-1
git merge feature/part-2
git merge feature/part-3
```

During merging, Git may report conflicts because multiple branches modify the same file.

## 4. Resolve Merge Conflicts

Inspect the conflicting files:

```bash
git status
```

Open the Python file:

```bash
nano flag.py
```

Git conflict markers look like:

```text
<<<<<<< HEAD
=======
>>>>>>> feature-branch
```

These markers indicate conflicting sections of code.

In this challenge, the conflicting code in `flag.py` was manually removed to produce a clean Python program.

Save the file and exit Nano.

## 5. Commit the Changes

Stage the modified files:

```bash
git add .
```

Commit the resolved changes:

```bash
git commit
```

## 6. Execute the Flag Printer

Run the Python program:

```bash
python3 flag.py
```

Output:

```text
Printing the flag...
academy{t3@mw0rk_m@k3s_th3_dr3@m_w0rk_e9d85913}
```

## Flag

```text
academy{t3@mw0rk_m@k3s_th3_dr3@m_w0rk_e9d85913}
```

## Key Concepts Learned

- Listing local Git branches using `git branch`.
- Understanding Git commit history and branch relationships.
- Merging feature branches into the main branch.
- Configuring Git user identity.
- Identifying and resolving merge conflicts.
- Editing conflicting files using Nano.
- Executing Python programs in Kali Linux.

## Conclusion

This challenge demonstrated how collaborative development works using Git branches and merges. By integrating the feature branches and resolving conflicts in the flag printing program, the final flag was successfully obtained.

**Lesson:** Teamwork makes the dream work!