# Issue

**Title:** Fix scientific tests

## Description

Attempt to fix some of the scientific tests having issues. 

This is complicated, in part, by not being able to replicate the exact issues on my local machine. So this isn't actually ready to be merged yet, but is being made as a PR solely to enable server testing debugging.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `80e656e7195c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.