# Issue

**Title:** Add warning for pose.cache security risks

## Description

Added warning in the PoseCacheAccessor class about the security risks of using the pickle module.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `df75a9c48e76`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.