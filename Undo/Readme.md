# Undo

## Objective

The goal of this challenge is to reverse a series of Linux text transformations and recover the original flag.

Connect to the challenge server using:

```bash
nc xebec.cylabacademy.net <LAB-ID>
```

At each step, the challenge provides a transformed flag and a hint. We need to identify the transformation and enter the Linux command that reverses it.

---

## Step 1 – Base64

### Current Flag

```text
KTZzMzU3MDUwLWZhMDFnQHplMHNmYTRlRy1nazNnLXRhMWZlcmlyRShsenJxbnBu
```

### Hint

```text
Base64 encoded the string.
```

The reverse operation for Base64 encoding is decoding.

### Command

```bash
base64 -d
```

The challenge then provides the next transformed string.

---

## Step 2 – Reverse the Text

### Current Flag

```text
)6s357050-fa01g@ze0sfa4eG-gk3g-ta1ferirE(lzrqnpn
```

### Hint

```text
Reversed the text.
```

The Linux `rev` command reverses the characters of a line.

### Command

```bash
rev
```

This produces:

```text
npnqrzl(Eriref1at-g3kg-Ge4afs0ez@g10af-050753s6)
```

---

## Step 3 – Replace Dashes with Underscores

### Current Flag

```text
npnqrzl(Eriref1at-g3kg-Ge4afs0ez@g10af-050753s6)
```

### Hint

```text
Replaced underscores with dashes.
```

The original transformation changed:

```text
_ → -
```

Therefore, we reverse it using `tr`:

```text
- → _
```

### Command

```bash
tr '-' '_'
```

Output:

```text
npnqrzl(Eriref1at_g3kg_Ge4afs0ez@g10af_050753s6)
```

---

## Step 4 – Replace Curly Braces with Parentheses

### Current Flag

```text
npnqrzl(Eriref1at_g3kg_Ge4afs0ez@g10af_050753s6)
```

### Hint

```text
Replaced curly braces with parentheses.
```

The original transformation was:

```text
{ → (
} → )
```

Therefore, we reverse it:

```text
( → {
) → }
```

Using `tr`, the command is:

```bash
tr '()' '{}'
```

Output:

```text
npnqrzl{Eriref1at_g3kg_Ge4afs0ez@g10af_050753s6}
```

---

## Step 5 – Reverse ROT13

### Current Flag

```text
npnqrzl{Eriref1at_g3kg_Ge4afs0ez@g10af_050753s6}
```

### Hint

```text
Applied ROT13 to letters.
```

ROT13 is its own inverse. Applying ROT13 a second time restores the original characters.

### Command

```bash
tr 'A-Za-z' 'N-ZA-Mn-za-m'
```

After applying ROT13, the final flag is:

```text
academy{Revers1ng_t3xt_Tr4nsf0rm@t10ns_050753f6}
```

---

# Final Flag

```text
academy{Revers1ng_t3xt_Tr4nsf0rm@t10ns_050753f6}
```

## Commands Used

| Step | Transformation  | Reverse Command              |
| ---- | --------------- | ---------------------------- |
| 1    | Base64 encoding | `base64 -d`                  |
| 2    | Text reversal   | `rev`                        |
| 3    | `_` → `-`       | `tr '-' '_'`                 |
| 4    | `{}` → `()`     | `tr '()' '{}'`               |
| 5    | ROT13           | `tr 'A-Za-z' 'N-ZA-Mn-za-m'` |

## Key Learning

The main concept in this challenge is **undoing transformations in reverse order**.

For `tr`, characters are mapped according to their positions. For example:

```bash
tr '()' '{}'
```

means:

```text
( → {
) → }
```

When reversing a transformation, the source and destination character sets need to be swapped.
