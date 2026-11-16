# Issue

**Title:** Minor changes to PyRosetta logging messages

## Description

Quick fixes to PyRosetta logging functionality:

- Change logging level when `pyrosetta.init(silent=True)`, which can really pollute logging files in `PyRosettaCluster` every time a new task is distributed to a dask worker (e.g., can be thousands of banners per log file).
- Do not print `conda` warnings on dask workers
- Typo in banner

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `cdd8f4763dd7`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.