# Issue

**Title:** Add -Wno-error for external libraries.

## Description

We shouldn't be doing warnings-as-errors for external libraries.

This change should make things more explicit.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `7e25e4a52fb9`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.