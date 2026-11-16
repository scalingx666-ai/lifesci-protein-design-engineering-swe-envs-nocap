# Issue

**Title:** fix arguments for MSMS computation

## Description

Adapt input arguemtns to updated versions of computeMSMS to ensure backward compativility in case someone wants to use the previous way of computing everything (via 01-odb_extract_and_triangulate.py etc.) even if not recommended.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `cdd6bc037c7d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.