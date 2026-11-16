# Issue

**Title:** fix data leakage bug: remove ipsae from RF features since it's the target

## Description

this looks cool! i don't think ipsae should be in the random forest features though since it's the target. lmk if i'm off base though

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `3f392106beed`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.