# Binary Search

## Challenge Information

- **Category:** General Skills
- **Difficulty:** Easy
- **Challenge Name:** Binary Search
- **Target Range:** 1–1000
- **Maximum Guesses:** 10
- **Objective:** Find the secret number using the shell-based guessing game.

## Challenge Description

The challenge provides an SSH connection to a guessing game. The server randomly selects a number between 1 and 1000, and the player must identify it within 10 guesses.

The game responds with `Higher!` or `Lower!` after each incorrect guess, providing clues to narrow down the search range.

## Connection Details

Connect to the challenge server using SSH:

```bash
ssh -p <TARGET-PORT> ctf-player@chatelaine.cylabacademy.net
```

Enter the provided challenge password when prompted and accept the SSH host fingerprint if necessary.

## Solution Approach

The challenge can be solved efficiently using the **Binary Search Algorithm**.

Binary search repeatedly divides a sorted search range into two halves, allowing us to eliminate approximately half of the remaining possibilities with each guess.

## Flag

```text
academy{g00d_gu355_35a2fc47}
```

## Key Takeaways

- Binary search is an efficient algorithm for narrowing down a search range.
- Feedback indicating whether a target is higher or lower helps eliminate impossible candidates.
- Choosing guesses strategically is more effective than guessing randomly.
- For a range of 1–1000, binary search can identify the target within 10 guesses.
- The secret number is randomly selected on each new connection, so the search must start again when reconnecting.

## Conclusion

The challenge was successfully completed by applying a binary-search strategy to narrow the possible values until the secret number `310` was found in seven guesses. This exercise demonstrates the importance of algorithmic thinking and efficient search techniques in command-line challenges.