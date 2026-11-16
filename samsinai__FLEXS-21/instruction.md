# Issue

**Title:** Add docstrings to explorers

## Description

There are a few modifications here aside from text-based ones:
- I removed the imports in `__init__.py` at the suggestion of flake8.
- I added `np.sqrt` to CMAES arg.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `663878d1c0b2`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.