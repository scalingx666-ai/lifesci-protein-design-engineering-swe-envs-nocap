# Issue

**Title:** Restoring the RandomGenerator state with PyRosetta initialization files

## Description

This quick PR supports:

1. Caching Rosetta's `RandomGenerator` Mersenne Twister (MT19937) internal state in a PyRosetta initialization file
2. Optionally restoring the MT19937 internal state when initializing from a PyRosetta initialization file

This enables continuity of the PyRosetta session's MT19937 internal state from the point at which the PyRosetta initialization file was written, independent of whether the `RandomGenerator` seed was explicitly configured.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `552382f5ef84`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.