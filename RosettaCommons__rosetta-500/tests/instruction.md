# Issue

**Title:** Change Gemmi submodule from ssh to https

## Description

Issue #492 indicates there's issues with having the Gemmi submodule loaded by SSH. Change that to HTTPS, to match other external submodules.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `7b35872d6974`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.