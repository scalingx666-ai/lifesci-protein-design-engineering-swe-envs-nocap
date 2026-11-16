# Issue

**Title:** Validation of number of CPUs/GPUs requested

## Description

Adds validation of CPU/GPU parameters to ensure multi-thread processes still work.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `9c2e6ede61a5`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.