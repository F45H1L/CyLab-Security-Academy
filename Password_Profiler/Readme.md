# Password Profiler

## Challenge Information

- **Category:** General Skills
- **Difficulty:** Easy
- **Challenge:** Password Profiler
- **Objective:** Recover a password from its SHA-1 hash using personal information and a custom wordlist.

## 1. Challenge Overview

The challenge provides three files:

| File | Description |
|---|---|
| `userinfo.txt` | Contains personal details about the target. |
| `hash.txt` | Contains the SHA-1 hash of the target's password. |
| `check_password.py` | Python script that checks a candidate password against the challenge hash. |

The goal is to use OSINT-inspired password profiling techniques to generate likely passwords and identify the candidate that matches the supplied hash.

## 2. Install CUPP

Clone the official CUPP repository:

```bash
git clone https://github.com/Mebus/cupp.git
cd cupp
```

CUPP stands for **Common User Passwords Profiler**. It generates personalized password dictionaries from information about a target.

Repository: https://github.com/Mebus/cupp

## 2. Download the Challenge Files

Download the files:

```bash
wget -O userinfo.txt "https://challenge-files.cylabacademy.net/library/eee2a66921bd7ab65c6464f5293a9d15cc01c8631a0dc4c9668db8a2525639fc/userinfo.txt"

wget -O hash.txt "https://challenge-files.cylabacademy.net/library/eee2a66921bd7ab65c6464f5293a9d15cc01c8631a0dc4c9668db8a2525639fc/hash.txt"

wget -O check_password.py "https://challenge-files.cylabacademy.net/library/eee2a66921bd7ab65c6464f5293a9d15cc01c8631a0dc4c9668db8a2525639fc/check_password.py"
```

Inspect the downloaded files:

```bash
cat userinfo.txt
cat hash.txt
cat check_password.py
```

## 3. Analyze the Target Information

The contents of `userinfo.txt` were:

```text
First Name: Alice
Surname: Johnson
Nickname: AJ
Birthdate: 15-07-1990
Partner's Name: Bob
Child's Name: Charlie
```

These details provide potential password components, including:

- First name: `Alice`
- Surname: `Johnson`
- Nickname: `AJ`
- Birthdate: `15071990`
- Partner's name: `Bob`
- Child's name: `Charlie`

Users sometimes combine names, nicknames, and dates to create passwords. CUPP automates the generation of likely combinations from these details.

## 5. Generate a Custom Wordlist

Launch CUPP's interactive mode:

```bash
python3 cupp.py -i
```

Enter the information obtained from `userinfo.txt`:

```text
First Name: Alice
Surname: Johnson
Nickname: AJ
Birthdate: 15071990
Partner's name: Bob
Child's name: Charlie
```

Leave unknown fields blank.

For this run, the following optional features were disabled:

- Additional keywords
- Special characters at the end of words
- Random numbers at the end of words
- Leetspeak substitutions

CUPP generated the following output:

```text
[+] Now making a dictionary...
[+] Sorting list and removing duplicates...
[+] Saving dictionary to alice.txt, counting 5252 words.
```

The resulting `alice.txt` file contained **5,252 candidate passwords**.

## 6. Prepare the Wordlist

A copy of the generated dictionary was created:

```bash
cp alice.txt passwords.txt
```

This preserves the original generated wordlist while providing a separate filename for the challenge workflow.

## 7. Verify a Candidate Password

The supplied Python verifier was executed with a test argument:

```bash
python3 check_password.py 'candidate_password'
```

The script returned:

```text
Password found: academy{aliceaj7515}
```

The verifier identified the successful challenge password.

## 8. Recovered Flag

```text
academy{aliceaj7515}
```

**Flag:** `academy{aliceaj7515}`

## 9. Why the Solution Worked

The solution uses a dictionary-based password recovery approach:

1. **Information gathering:** Extract the target's personal details from `userinfo.txt`.
2. **Password profiling:** Use CUPP to generate plausible password combinations.
3. **Dictionary generation:** Produce a list of 5,252 candidates.
4. **Hash verification:** Use the supplied Python script to determine which candidate matches the challenge's stored hash.
5. **Flag recovery:** Retrieve the expected challenge flag from the verifier's successful output.

The password `aliceaj7515` incorporates the target's first name, nickname initials, and a numeric suffix. The exact construction illustrates how personal information can make a password more predictable.

## 10. Lessons Learned

- Personal information can make passwords easier to predict.
- Custom wordlists can be more effective than generic dictionaries when a target's details are known.
- CUPP automates the generation of personalized password candidates.
- Hash verification lets an investigator test candidates without needing to know the original password in advance.
- SHA-1 is a cryptographic hash function, not encryption; a hash cannot generally be reversed directly, but candidate passwords can be hashed and compared.
- In real applications, passwords should be stored using a dedicated password-hashing algorithm such as Argon2id, scrypt, or appropriately configured bcrypt, with unique salts.

## Conclusion

The Password Profiler challenge was solved by collecting the supplied personal information, generating a custom dictionary with CUPP, and using the provided verification script to recover the expected flag:

`academy{aliceaj7515}`

This exercise demonstrates the importance of avoiding passwords derived from names, nicknames, family information, or predictable number patterns.