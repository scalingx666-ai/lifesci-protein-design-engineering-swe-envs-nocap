# Issue

**Title:** Defining a PyRosetta build signature specifying extras

## Description

This quick PR defines a unique PyRosetta build signature that differs from the existing PyRosetta version string in that it contains build extras as given by `rosetta.utility.Version.extras()`. The aim is to capture build differences in the official PyRosetta `ml` images from non-`ml` images. The `PyRosettaCluster(pyrosetta_build=...)` default keyword argument parameter, and the PyRosetta `.init` file `pyrosetta_build` key value, are updated herein accordingly.

Here is an example of the new `pyrosetta._build_signature()` function's output:
```
PyRosetta4.Release.python311.mac.cxx11thread.serialization[extras:cxx11thread+serialization]2025.19+release.1354d05daa4c339d591afeecef3c94ca2d38680e
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `de3cc17d5092`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.