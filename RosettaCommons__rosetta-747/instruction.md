# Issue

**Title:** Caching environment manager version in PyRosettaCluster full simulation records

## Description

This quick PR adds a caching mechanism for the configured environment manager's version string in the *PyRosettaCluster* full simulation record. The captured version string facilitates environment reproducibility when using Pixi or uv, as described in the accompanying PR:

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `a19f074e0150`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.