# bytemancy 0

## Challenge Information

* **Category:** General Skills
* **Difficulty:** Easy
* **Challenge:** Bytemancy 0
* **Server:** `chatelaine.cylabacademy.net`
* **Port:** `16070`

## Description

The challenge asks us to provide the ASCII characters represented by the given decimal ASCII values.

The server displays:

```text
Send me ASCII DECIMAL 101, 101, 101, side-by-side, no space.
```

The important part is understanding that `101` is an **ASCII decimal value**, not the literal text `101`.

## Step 1 — Get the Source Code

The challenge provides the Python source code at:

```text
https://challenge-files.cylabacademy.net/library/f2817de8294234b67b3f4bee024ebf84cb69b59a6f66682ff69f566234394dec/app.py
```

Download and display it directly:

```bash
wget -qO- 'https://challenge-files.cylabacademy.net/library/f2817de8294234b67b3f4bee024ebf84cb69b59a6f66682ff69f566234394dec/app.py'
```

Alternatively, save it locally:

```bash
wget -O app.py 'https://challenge-files.cylabacademy.net/library/f2817de8294234b67b3f4bee024ebf84cb69b59a6f66682ff69f566234394dec/app.py'
```

Then:

```bash
cat app.py
```

## Step 2 — Understand the ASCII Value

The challenge gives:

```text
101, 101, 101
```

These are decimal ASCII codes.

ASCII decimal `101` corresponds to:

```text
101 → e
```

Since the challenge asks for the characters **side-by-side with no spaces**, the required answer is:

```text
eee
```

## Step 3 — Connect to the Challenge

Connect using netcat:

```bash
nc chatelaine.cylabacademy.net <LAB-ID>
```

When prompted:

```text
Send me ASCII DECIMAL 101, 101, 101, side-by-side, no space.
```

Enter:

```text
eee
```

## Step 4 — One-Liner

The challenge hint says:

> Solving this with a one-liner will help with the next challenge in this series.

We can send the answer directly to the server using:

```bash
printf 'eee\n' | nc chatelaine.cylabacademy.net <LAB-ID>>
```

This avoids manually entering the answer.

## Key Takeaway

When a challenge asks for an **ASCII decimal value**, convert the number into its corresponding ASCII character.

For example:

```text
101 → e
101 → e
101 → e
```

Therefore:

```text
101, 101, 101 → eee
```

The final answer is:

```text
eee
```

## Useful Commands

### Download source

```bash
wget -qO- 'https://challenge-files.cylabacademy.net/library/f2817de8294234b67b3f4bee024ebf84cb69b59a6f66682ff69f566234394dec/app.py'
```

### Connect manually

```bash
nc chatelaine.cylabacademy.net <LAB-ID>
```

### Submit using a one-liner

```bash
printf 'eee\n' | nc chatelaine.cylabacademy.net <LAB-ID>
```