# Issue

**Title:** Use new environment in readme and fix mkl version bug

## Description

The goal of #2 was to greatly simplify the installation by reducing the number of commands someone needs to run to create the environment. This updates to readme to use the minimal commands. These are the same commands that run in [GitHub Actions](https://github.com/chao1224/ProteinDT/actions).

I still haven't tested on a GPU though.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `369c3cefbb2b`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.