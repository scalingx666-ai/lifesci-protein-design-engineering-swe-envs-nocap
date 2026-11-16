# Issue

**Title:** Add pickle warning message to PackedPose class docstring.

## Description

The pickle module has some inherent security issues, see https://docs.python.org/3/library/pickle.html.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `df75a9c48e76`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.