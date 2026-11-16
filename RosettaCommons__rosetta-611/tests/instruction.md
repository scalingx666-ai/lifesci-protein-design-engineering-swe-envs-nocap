# Issue

**Title:** Update SecureUnpickler disallowed packages

## Description

This PR updates the `pyrosetta.secure_unpickle.SecureUnpickler` class to block some additional callable targets via the `pickle` module, including `numpy.load` and `pandas.read_pickle` modules. Unit tests added herein demonstrate that secure `numpy`/`pandas` modules like `numpy.array` and `pandas.DataFrame` are still deserializable.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `1a56185c2592`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.