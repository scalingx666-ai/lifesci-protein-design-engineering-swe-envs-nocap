# Issue

**Title:** Fix: adjust TMPDIR path and update PYTHONPATH export

## Description

- Made minor adjustments to solve reported issues (#1 and #2) in the file `preprocess_pdb.sh`
- Added a `.gitignore` file to ignore generated examples

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `6ce860a673ef`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.