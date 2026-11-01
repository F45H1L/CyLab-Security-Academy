# endianness

**Category:** General Skills
**Difficulty:** Easy
**Challenge:** endianness
**Target:** `xebec.cylabacademy.net:<TARGET-PORT`

## 1. Challenge Description

The challenge introduces the concept of Little Endian and Big Endian byte ordering.

The server generates a random five-character lowercase word and asks us to provide:

1. Little Endian hexadecimal representation.
2. Big Endian hexadecimal representation.

If both representations are correct, the server reveals the flag.

## 2. Source Code Analysis

After downloading the provided `flag.c` source file, we identified two important functions.

### Little Endian

```c
char *find_little_endian(const char *word)
{
    size_t word_len = strlen(word);
    char *little_endian = malloc((2 * word_len + 1));

    for (size_t i = word_len; i-- > 0;)
    {
        snprintf(&little_endian[(word_len - 1 - i) * 2],
                 3, "%02X", (unsigned char)word[i]);
    }

    little_endian[2 * word_len] = '\0';
    return little_endian;
}
```

This function iterates through the input word in reverse order and converts each character into its two-digit hexadecimal ASCII representation.

### Big Endian

```c
char *find_big_endian(const char *word)
{
    size_t length = strlen(word);
    char *big_endian = malloc((2 * length + 1));

    for (size_t i = 0; i < length; i++)
    {
        snprintf(&big_endian[i * 2], 3,
                 "%02X", (unsigned char)word[i]);
    }

    big_endian[2 * length] = '\0';
    return big_endian;
}
```

This function iterates through the word in its original order and converts each character into hexadecimal.

## 3. Understanding Endianness

Endianness determines the order in which bytes are represented.

- **Big Endian:** Most significant byte comes first.
- **Little Endian:** Least significant byte comes first.

For example, consider the word `apple`.

| Character | ASCII Hex |
| --------- | --------- |
| a         | 61        |
| p         | 70        |
| p         | 70        |
| l         | 6C        |
| e         | 65        |

The resulting representations are:

**Big Endian:**

```text
6170706C65
```

**Little Endian:**

```text
656C707061
```

Notice that Little Endian reverses the byte sequence, not the individual hexadecimal digits.

## 4. Exploitation / Solution Steps

### Step 1: Connect to the challenge

We connected to the remote challenge using Netcat.

```bash
nc xebec.cylabacademy.net 17901
```

### Step 2: Identify the generated word

The server generated the following random word:

```text
Word: ouyob
```

### Step 3: Convert ASCII characters into hexadecimal

We converted each character using its ASCII hexadecimal representation.

| Character | ASCII Hex |
| --------- | --------- |
| o         | 6F        |
| u         | 75        |
| y         | 79        |
| o         | 6F        |
| b         | 62        |

### Step 4: Calculate Little Endian

Reverse the byte sequence:

```text
Original: 6F 75 79 6F 62
Reversed: 62 6F 79 75 6F
```

Input:

```text
626F79756F
```

Server response:

```text
Correct Little Endian representation!
```

### Step 5: Calculate Big Endian

Keep the original byte sequence:

```text
6F 75 79 6F 62
```

Input:

```text
6F75796F62
```

Server response:

```text
Correct Big Endian representation!
```

### Step 6: Retrieve the flag

After successfully submitting both representations, the server returned:

```text
Congratulations! You found both endian representations correctly!
Your Flag is: academy{3ndi4n_sw4p_su33ess_c70286c6}
```

## 5. Python Automation

The conversion process can also be automated using Python.

```python
word = input("Enter the challenge word: ")

little_endian = word.encode()[::-1].hex().upper()
big_endian = word.encode().hex().upper()

print("Little Endian:", little_endian)
print("Big Endian:   ", big_endian)
```

This script:

- Encodes the word into bytes.
- Reverses the byte sequence for Little Endian.
- Converts both byte sequences into uppercase hexadecimal.

## 6. Final Flag

```text
academy{3ndi4n_sw4p_su33ess_c70286c6}
```

## 7. Key Takeaways

1. ASCII characters can be represented using hexadecimal values.
2. Big Endian preserves the original byte order.
3. Little Endian reverses the byte order.
4. Endianness changes byte ordering, not the individual bits or hexadecimal digits.
5. Python's `encode()`, slicing, and `hex()` functions make byte-order conversion straightforward.

**Challenge Status:** Solved