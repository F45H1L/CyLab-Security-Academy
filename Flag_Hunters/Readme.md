# Flag Hunters

## Challenge Information

- **Challenge Name:** Flag Hunters
- **Category:** Reverse Engineering
- **Difficulty:** Easy
- **Platform:** CyLab Academy
- **Connection:** `nc chatelaine.cylabacademy.net 10672`

## Challenge Description

The program prints song lyrics and supports a hidden refrain that is not intended to be printed by default. The objective is to manipulate the program's control flow to reveal the hidden content containing the flag.

The challenge provides the Python source code, `lyric-reader.py`, which can be inspected to understand how the lyrics are processed.

## 1. Source Code Analysis

Download the source code:

```bash
wget -O lyric-reader.py 'https://challenge-files.cylabacademy.net/library/51bc77a69fd2044e4670e5e7024778917093a32cb6db10f0ba4bef90d2005ae7/lyric-reader.py'
```

Inspect the source:

```bash
cat -n lyric-reader.py
```

### Flag initialization

The program reads the flag from a local file and concatenates it into the song's introduction.

```python
flag = open('flag.txt', 'r').read()

secret_intro = \
'''Pico warriors rising, puzzles laid bare,
Solving each challenge with precision and flair.
With unity and skill, flags we deliver,
The ether’s ours to conquer, '''\
+ flag + '\n'
```

This indicates that the flag is already embedded in the song string before the reader starts printing the lyrics.

### Refrain handling

The program identifies the location of the hidden refrain and the `RETURN` instruction:

```python
elif song_lines[i] == '[REFRAIN]':
    refrain = i + 1
elif song_lines[i] == 'RETURN':
    refrain_return = i
```

When the reader encounters `REFRAIN`, it saves a return address and jumps to the refrain:

```python
if line == 'REFRAIN':
    song_lines[refrain_return] = 'RETURN ' + str(lip + 1)
    lip = refrain
```

The return instruction changes the current line index:

```python
elif re.match(r"RETURN [0-9]+", line):
    lip = int(line.split()[1])
```

This creates an opportunity to manipulate the program's execution by injecting an instruction through user input.

## 2. Identifying the Vulnerability

The vulnerable code is:

```python
elif re.match(r"CROWD.*", line):
    crowd = input('Crowd: ')
    song_lines[lip] = 'Crowd: ' + crowd
    lip += 1
```

The program accepts user input and inserts it directly into a song line. Later, that line is split into separate instructions using semicolons:

```python
for line in song_lines[lip].split(';'):
```

Because the input is not sanitized, a semicolon can introduce an additional instruction that the program will interpret.

The relevant weaknesses are:

- Unsanitized user input.
- Semicolon-delimited instruction parsing.
- Unrestricted numeric return targets.
- No validation that a return target belongs to the intended part of the song.

## 3. Exploitation

Connect to the remote challenge:

```bash
nc chatelaine.cylabacademy.net 10672
```

Wait until the program prompts:

```text
Crowd:
```

Enter the following payload:

```text
;RETURN 0
```

### Why the payload works

The application prepends `Crowd: ` to the supplied input. The resulting line is:

```text
Crowd: ;RETURN 0
```

The semicolon splits this into two parts:

```text
Crowd:
RETURN 0
```

The first part is printed as ordinary text. The second part matches the return-instruction regular expression:

```python
re.match(r"RETURN [0-9]+", line)
```

The program then executes:

```python
lip = int(line.split()[1])
```

Since the injected target is `0`, execution jumps to the beginning of `song_lines`.

The first lines contain `secret_intro`, which includes the flag. Consequently, the program prints the hidden introduction and reveals the flag.

## 4. Captured Flag

```text
academy{70637h3r_f0r3v3r_4943a085}
```

## 5. Why the Output Repeats

After printing the secret introduction, the program does not immediately terminate. It continues processing the song, which can lead to the refrain being executed again.

The `RETURN 0` payload redirects execution to the beginning of the song, but it does not set the `finished` variable to `True`. The reader therefore continues until it encounters `END` or reaches the `MAX_LINES` limit.

The repeated flag output is a side effect of the manipulated control flow, not a requirement for successful exploitation.

## 6. Root Cause

The vulnerability results from treating user-controlled text as part of a custom instruction language without enforcing proper boundaries between data and instructions.

The program combines:

1. Direct insertion of untrusted input into the song.
2. Semicolon-based instruction separation.
3. Regex-based instruction recognition.
4. User-influenced control flow through numeric return targets.

Together, these behaviors allow a user to inject a valid `RETURN` instruction and redirect execution to an earlier part of the song.

## 7. Mitigation

To prevent this class of vulnerability:

- Treat user input strictly as data, never as executable instructions.
- Escape or reject instruction delimiters in user-controlled content.
- Keep the lyric data separate from the instruction parser.
- Validate return addresses against an explicit set of permitted locations.
- Replace the custom text-based instruction mechanism with structured control flow.
- Add tests for delimiter injection, invalid return addresses, and unexpected input.

## Conclusion

The Flag Hunters challenge demonstrates how seemingly harmless input handling can introduce control-flow vulnerabilities in a custom interpreter.

By inspecting the Python source, identifying the semicolon-based instruction parser, and injecting `;RETURN 0` at the crowd prompt, it was possible to redirect execution to the beginning of the song and disclose the embedded flag.

**Final flag:** `academy{70637h3r_f0r3v3r_4943a085}`