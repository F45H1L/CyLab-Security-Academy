# MY GIT

**Category:** General Skills
**Difficulty:** Easy

## Description

The challenge provides a custom Git server and asks us to push `flag.txt` to obtain the flag.

The important clue is in the hint:

> How do you specify your Git username and email?

## Step 1 — Clone the Repository

Clone the challenge repository using the provided command:

```bash
git clone ssh://git@chatelaine.cylabacademy.net:<LAB-ID>/git/challenge.git
```

When prompted for the password, enter:

```text
abafae1c
```

Then enter the repository:

```bash
cd challenge
```

## Step 2 — Read the README

Check the repository README:

```bash
cat README.md
```

It contains:

```text
If you want the flag, make sure to push the flag!

Only flag.txt pushed by root:root@academy will be updated with the flag.
```

This tells us that the Git commit must be made using:

```text
Username: root
Email: root@academy
```

## Step 3 — Check the Git Configuration

Check the current Git configuration:

```bash
git config --list
```

The relevant values were:

```text
user.name=root
user.email=root@academy
```
Check the remote URL;
```bash
git remote -v
```
You should see:
```
origin  ssh://git@chatelaine.cylabacademy.net:<LAB-ID/git/challenge.git (fetch)
origin  ssh://git@chatelaine.cylabacademy.net:<LAB-ID>/git/challenge.git (push)
```
So the Git identity already matches the requirement.

If they are not configured correctly, set them with:

```bash
git config user.name "root"
git config user.email "root@academy"
```

Verify:

```bash
git config user.name
git config user.email
```

Expected output:

```text
root
root@academy
```
Check the remote URL;
```bash
git remote -v
```
You should see 
```
origin  ssh://git@chatelaine.cylabacademy.net:<LAB-ID/git/challenge.git (fetch)
origin  ssh://git@chatelaine.cylabacademy.net:<LAB-ID>/git/challenge.git (push)
```
So the Git identity already matches the requirement.

If they are not configured correctly, set them with:
```bash
git remote set-url origin 'ssh://git@<CURRENT_HOST>:<CURRENT_PORT>/git/challenge.git'
```

## Step 4 — Create `flag.txt`

Create the required file:

```bash
echo "flag" > flag.txt
```

The actual contents don't need to be the flag. The custom Git server will update the file when it receives a valid push from the required Git identity.

## Step 5 — Commit the File

Stage the file:

```bash
git add flag.txt
```

Commit it:

```bash
git commit -m "flag"
```

Example:

```text
[master ebd1ac5] flag
 1 file changed, 1 insertion(+)
 create mode 100644 flag.txt
```

## Step 6 — Push the Commit

Push the commit to the challenge server:

```bash
git push origin master
```

The server checks the Git author information.

Because the commit was created with:

```text
root:root@academy
```

the server updates `flag.txt` with the actual flag.

## Step 7 — Read the Flag

After a successful push:

```bash
cat flag.txt
```

The flag should now be displayed.

## Important Note

If you receive:

```text
ssh: connect to host ... port ...: Connection refused
```

the challenge instance may have expired or restarted.

Check the active picoCTF instance and use its **current clone URL and port**. The remote can be updated with:

```bash
git remote set-url origin 'ssh://git@<CURRENT_HOST>:<CURRENT_PORT>/git/challenge.git'
```

Then push again:

```bash
git push origin master
```

## Flag

```text
<paste your flag here>
```

## Key Takeaway

The challenge demonstrates that Git commits contain author identity information. The server uses the commit's `user.name` and `user.email` to determine whether `flag.txt` should be updated.

The crucial identity was:

```text
user.name = root
user.email = root@academy
```