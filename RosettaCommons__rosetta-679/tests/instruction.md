# Issue

**Title:** Document mpistream rank-zero routing

## Description

## Summary
- document that mpistream write messages intentionally route through rank 0

## Testing
- python3 ./scons.py -j16 mode=debug

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `cf532f89b6bb`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.