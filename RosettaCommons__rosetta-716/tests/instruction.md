# Issue

**Title:** Fix the extras=bcl compilation.

## Description

I accidentally broke it with the multi-chainletter PR #551

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `bbb6a2d27c70`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.