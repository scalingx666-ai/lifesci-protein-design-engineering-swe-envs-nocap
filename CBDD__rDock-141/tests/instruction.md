# Issue

**Title:** Issue #140/improve installation process

## Description

with this PR, we now copy the data folder into `${RBT_ROOT}` if provided or `${PREFIX}/shared/rdock/data` otherwise.

i also took the chance to update the makefile documentation and the readme to reflect the new changes.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `25b2ad59829d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.