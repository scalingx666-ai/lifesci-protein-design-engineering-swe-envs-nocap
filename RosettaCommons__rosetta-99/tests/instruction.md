# Issue

**Title:** Made hbond graph pyrosetta compatible

## Description

You can't actually use the HBondGraph in pyrosetta because the LowMemGraph uses some crazy tricks that don't involve object inheritance.

This adds 2 functions to bridge that gap.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `9ee00de6d541`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.