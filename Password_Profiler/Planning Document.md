# Planning Document — Password Profiler

## 1. Challenge Overview

- **Platform:** CyLab Academy
- **Challenge Name:** Password Profiler
- **Category:** General Skills
- **Difficulty:** Easy

### Challenge Description

A suspicious file contains the SHA-1 hash of a password rather than the password itself. Personal information about the target is provided to help generate a customized password dictionary.

The objective is to use OSINT-inspired password profiling techniques to generate likely password candidates and identify the original password by comparing candidates against the supplied hash.

## 2. Objectives

The primary objectives of this investigation are:

1. Download and inspect the challenge files.
2. Identify useful password-related information in the target's personal details.
3. Understand how the provided password verification script works.
4. Set up CUPP (Common User Passwords Profiler) to generate a custom wordlist.
5. Test generated password candidates against the supplied SHA-1 hash.
6. Identify the correct password and retrieve the challenge flag.
7. Document the methodology, commands, findings, and lessons learned.

## 3. Provided Resources

The challenge provides the following files:

| File | Purpose |
|---|---|
| `userinfo.txt` | Contains personal information about the target. |
| `hash.txt` | Contains the SHA-1 hash to be matched. |
| `check_password.py` | Python script supplied to verify candidate passwords. |

The challenge also provides a hint recommending CUPP for generating custom password dictionaries.

## 4. Tools and Technologies

The following tools and utilities are planned for the investigation:

- **Kali Linux:** Operating environment for the investigation.
- **CUPP:** Generates personalized password dictionaries from target information.
- **Python 3:** Executes the supplied verification script and supports automation if needed.
- **Git:** Downloads the CUPP source code.
- **Wget:** Downloads the challenge files.
- **Linux terminal utilities:** Inspect files and manage the generated wordlist.

## 5. Planned Methodology

### Phase 1: Environment Preparation

Create a dedicated working directory to keep the challenge files and generated wordlists organized.

Planned actions:

- Create a directory for the Password Profiler challenge.
- Verify Python 3 and Git availability.
- Download the three supplied challenge files.
- Confirm that the files were downloaded successfully.

Expected outcome: A working environment containing all required challenge resources.

### Phase 2: Initial File Analysis

Inspect the supplied files before attempting password recovery.

Planned commands:

```bash
cat userinfo.txt
cat hash.txt
cat check_password.py
```

The investigation will determine:

- Which personal details may be useful for password profiling.
- Whether the hash file contains a valid SHA-1 digest.
- How the verification script receives candidate passwords.
- How the script determines whether a candidate is correct.
- Whether the script prints a flag or a success message upon a match.

Expected outcome: An understanding of the target information, hash format, and verification process.

### Phase 3: CUPP Setup

Obtain CUPP from its official repository.

Repository:

https://github.com/Mebus/cupp

Planned commands:

```bash
git clone https://github.com/Mebus/cupp.git
cd cupp
python3 cupp.py -i
```

The interactive mode will be used to enter the available information from `userinfo.txt`.

Only information provided by the challenge will be used. Unknown fields will be left blank rather than guessed.

Expected outcome: A personalized password dictionary containing likely combinations of names, dates, nicknames, and other supplied details.

### Phase 4: Wordlist Generation and Review

Generate the custom wordlist using CUPP.

The generated file will be inspected to confirm that it exists and contains candidate passwords.

Planned commands:

```bash
ls -lh
wc -l alice.txt
head alice.txt
```

The filename shown above is an example; the actual filename will depend on the information entered into CUPP.

Additional wordlist variations may be considered if the initial dictionary does not produce a match. These may include numeric suffixes, special characters, or leetspeak transformations.

Expected outcome: A verified wordlist ready for password candidate testing.

### Phase 5: Password Verification

Examine the supplied Python script to determine its expected input format and matching behavior.

The candidate-testing process will then be selected accordingly.

Possible approaches include:

- Testing individual candidates manually.
- Using a script to iterate through the generated wordlist.
- Comparing SHA-1 digests directly if the verifier's implementation supports or warrants that approach.

Any automated testing must use the actual verification logic and a reliable success condition. A printed message alone should not be treated as proof until the script's behavior has been examined.

Expected outcome: Identification of a candidate that is confirmed to match the challenge's expected password.

### Phase 6: Flag Retrieval

After identifying a valid candidate:

- Confirm the match using the supplied verifier.
- Record the successful output.
- Extract the flag in the format expected by CyLab Academy.
- Preserve the evidence required to reproduce the result.

Expected outcome: A verified challenge flag and a reproducible solution.

### Phase 7: Documentation

Prepare the final challenge write-up with the following sections:

- Challenge overview
- Tools used
- Initial file analysis
- CUPP installation and configuration
- Wordlist generation
- Password verification
- Recovered flag
- Technical explanation
- Lessons learned

The final report will distinguish between planned steps and actions actually completed.

## 6. Investigation Workflow

```text
Start
  |
  v
Download challenge files
  |
  v
Inspect personal details, hash, and verifier
  |
  v
Configure CUPP with available information
  |
  v
Generate a custom password dictionary
  |
  v
Test candidates against the expected hash
  |
  v
Did a candidate match?
  |             |
  No            Yes
  |              |
  v              v
Review wordlist  Confirm candidate
and strategy     using the verifier
  |              |
  └── Retry      v
             Retrieve flag
                  |
                  v
             Document findings
                  |
                  v
                 End
```

## 7. Potential Challenges

The investigation may encounter the following issues:

- **Insufficient personal information:** The initial wordlist may not contain the correct password.
- **Incorrect date format:** Dates entered in the wrong format may reduce the relevance of generated candidates.
- **Wordlist limitations:** Disabled suffix, special-character, or leetspeak options may exclude plausible combinations.
- **Verifier input mismatch:** The supplied script may expect a specific command-line argument or input format.
- **False success detection:** An automation script may misinterpret an output message as a successful match.
- **Hash interpretation:** Extra whitespace or an incorrect assumption about the stored digest may cause verification problems.

Each issue will be investigated before modifying the recovery strategy.

## 8. Security Considerations

This activity is restricted to the files and verification mechanism provided by the authorized CyLab Academy challenge.

The exercise is intended to demonstrate password predictability and the risks of incorporating publicly discoverable personal details into passwords.

No external accounts or unrelated systems will be targeted.

## 9. Expected Deliverables

Upon completion, the investigation should produce:

1. A custom password dictionary generated by CUPP.
2. Evidence of the candidate-verification process.
3. The confirmed challenge flag.
4. A `README.md` containing the reproducible solution.
5. A final report explaining the methodology and technical findings.

## 10. Success Criteria

The challenge will be considered successfully completed when:

- The supplied files have been inspected.
- CUPP has generated a custom password dictionary.
- A candidate has been verified using the intended challenge logic.
- The expected flag has been recovered.
- The steps and findings have been documented accurately.

## Conclusion

This planning document establishes a structured approach to solving the Password Profiler challenge. The investigation will begin with file analysis, proceed to personalized wordlist generation with CUPP, and conclude with hash verification and evidence-based documentation.

No password or flag is assumed in advance; all conclusions will be based on the results obtained during the investigation.