# Report — Password Profiler

## 1. Executive Summary

The Password Profiler challenge is a General Skills exercise on CyLab Academy designed to demonstrate how personal information can be used to generate likely password candidates.

The challenge provides a file containing personal details about a target, a file containing the SHA-1 hash of a password, and a Python script for checking candidate passwords.

The planned investigation will use CUPP (Common User Passwords Profiler) to generate a customized password dictionary from the available information. The resulting candidates will then be tested against the supplied verification mechanism to identify the password associated with the hash.

At this stage, the investigation has not begun, and no password or flag has been confirmed.

## 2. Challenge Details

| Attribute | Details |
|---|---|
| Platform | CyLab Academy |
| Challenge name | Password Profiler |
| Category | General Skills |
| Difficulty | Easy |
| Primary technique | Custom password dictionary generation |
| Hash algorithm | SHA-1 |
| Recommended tool | CUPP |
| Environment | Kali Linux |

## 3. Problem Statement

The challenge describes a scenario in which a suspicious file contains a SHA-1 password hash instead of the original password.

Personal information about the target is provided to support an OSINT-inspired password recovery process. The objective is to use this information to construct likely password candidates and determine which candidate matches the supplied hash.

The challenge emphasizes the relationship between personal information, password predictability, and dictionary-based password recovery.

## 4. Objectives

The investigation aims to:

1. Retrieve and inspect the supplied challenge files.
2. Identify personal details that may influence password construction.
3. Examine the Python verification script to understand its input and matching logic.
4. Install and configure CUPP in Kali Linux.
5. Generate a customized password dictionary.
6. Test generated candidates against the expected hash.
7. Confirm the correct password using the supplied verification mechanism.
8. Record the findings and document the recovered challenge flag.

## 5. Resources Provided

The challenge provides three files.

### 5.1 `userinfo.txt`

This file contains personal information about the target. The details may include names, nicknames, dates, or other information that could be incorporated into a password.

The information will be examined to determine which fields are relevant to password profiling.

### 5.2 `hash.txt`

This file contains the SHA-1 hash associated with the password being investigated.

The hash will serve as the reference value against which candidate passwords are checked.

### 5.3 `check_password.py`

This Python script is supplied to test password candidates.

Its implementation will be inspected to establish how passwords are supplied, how the hash comparison is performed, and how successful verification is reported.

## 6. Tools and Technologies

The following tools are planned for the investigation:

| Tool | Purpose |
|---|---|
| Kali Linux | Security testing environment |
| CUPP | Generate personalized password candidates |
| Python 3 | Execute the supplied verification script |
| Git | Obtain the CUPP source code |
| Wget | Download challenge files |
| Linux command-line utilities | Inspect files and manage wordlists |

CUPP repository: https://github.com/Mebus/cupp

## 7. Methodology

### Phase 1: Environment Preparation

A dedicated working directory will be created to organize the challenge files, generated dictionary, and supporting evidence.

The required files will be downloaded from the official challenge-provided links. Their presence and contents will be verified before proceeding.

### Phase 2: Initial Analysis

The contents of `userinfo.txt`, `hash.txt`, and `check_password.py` will be examined.

The analysis will focus on identifying useful personal details, determining the hash format, and understanding the verifier's behavior.

This phase will establish the information needed to configure the wordlist generator and select the correct verification procedure.

### Phase 3: Custom Wordlist Generation

CUPP will be used in interactive mode to generate likely passwords from the supplied personal details.

The process will consider combinations of names, nicknames, dates, and other available information. Optional transformations may be explored if the initial dictionary is insufficient.

The generated wordlist will be checked to confirm that it exists and contains candidate passwords.

### Phase 4: Candidate Verification

The generated candidates will be tested using the challenge's verification mechanism.

The verification process will be based on the actual behavior of `check_password.py`, rather than assumptions about its command-line interface or success messages.

Where appropriate, a controlled script may automate testing of the generated dictionary.

### Phase 5: Result Validation

Any candidate reported as successful will be independently checked against the intended hash-verification logic.

The output will be reviewed to ensure that the reported result represents a genuine match rather than a hardcoded message or unrelated script behavior.

### Phase 6: Documentation

The final findings will be recorded, including the tools used, commands executed, wordlist-generation process, verification evidence, and recovered flag.

Only findings supported by the investigation will be included in the completed report.

## 8. Technical Background

### 8.1 SHA-1 Hashing

SHA-1 is a cryptographic hash function that produces a 160-bit digest, commonly represented as 40 hexadecimal characters.

Hashing is a one-way operation; it is not equivalent to encryption, which is designed to be reversible with the appropriate key.

Password recovery therefore typically involves generating candidate passwords, calculating their hashes using the appropriate procedure, and comparing the results with the stored digest.

### 8.2 Personalized Password Dictionaries

A personalized password dictionary contains candidates derived from information associated with a specific target.

CUPP automates the generation of these candidates and may apply combinations and transformations to the supplied information.

This approach can be effective when passwords are based on predictable personal details, although its effectiveness depends on whether the actual password construction is represented in the generated dictionary.

## 9. Potential Challenges

Several factors may affect the investigation:

- Missing personal details may reduce the usefulness of the generated wordlist.
- Date-format differences may result in relevant candidates being omitted.
- Password suffixes, special characters, or letter substitutions may require additional wordlist variations.
- The verification script may require a specific input format.
- Automated testing may incorrectly identify a candidate if success conditions are not interpreted correctly.
- The challenge's implementation may require additional analysis if the expected verification behavior differs from initial assumptions.

These issues will be investigated as necessary.

## 10. Security Implications

The challenge demonstrates that passwords constructed from names, nicknames, family details, and dates may be vulnerable to targeted guessing.

Organizations and individuals should avoid using predictable personal information as the primary basis for passwords.

Applications should not rely on fast general-purpose hash functions such as SHA-1 for password storage. Dedicated password-hashing algorithms, including Argon2id, scrypt, or appropriately configured bcrypt, are designed to make large-scale password guessing more expensive.

## 11. Findings and Evidence

**Investigation status:** Not started.

The following items remain pending:

| Item | Status |
|---|---|
| Challenge files downloaded and inspected | Pending |
| Personal details analyzed | Pending |
| Verification script examined | Pending |
| CUPP configured | Pending |
| Custom wordlist generated | Pending |
| Password candidate verified | Pending |
| Challenge flag recovered | Pending |
| Final evidence collected | Pending |

No recovered password or flag is reported at this stage.

## 12. Conclusion

The Password Profiler challenge will be approached through structured file analysis, personalized wordlist generation, and candidate verification.

CUPP will provide the primary mechanism for generating password candidates from the supplied personal information. The challenge-provided Python script will be examined to ensure that candidate testing follows the intended verification logic.

The investigation will be considered complete once a password candidate has been validated against the expected hash, the challenge flag has been recovered, and the results have been documented with reproducible evidence.