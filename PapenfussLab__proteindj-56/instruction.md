# Issue

**Title:** deterministic metadata merge order

## Description

This  by enforcing a deterministic resolution of the `description` field when merging metadata across pipeline stages.

Resolution order is:
pr > boltz > af2 > mpnn = fampnn = seq

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `32d8e90589d4`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.