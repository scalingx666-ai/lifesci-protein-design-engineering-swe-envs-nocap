# Issue

**Title:** Supporting mamba, uv, and pixi environment managers in PyRosettaCluster

## Description

`PyRosettaCluster` currently uses the `conda` environment manager for simulation reproduction life cycles. Herein this PR, we add support for `mamba`, `uv`, and `pixi` as well. Upon instantiation of `PyRosettaCluster`, we now look for the operating system environment variable `PYROSETTACLUSTER_ENVIRONMENT_MANAGER` to determine if the user wants to use a certain environment manager, or otherwise fallback on detecting whether `pixi`, `uv`, `mamba`, or `conda` is an executable to determine which environment manager to use. A second party reproducing a simulation may then use the same or a different environment manager, since the raw YAML file string is universal across environment managers and is cached in the `PyRosettaCluster` decoy output file and/or scorefile. Note that unit tests are not added herein because the Benchmark server implements virtual environments (i.e., using `pip`), and therefore these new functionalities are not testable at the moment (and so they may be considered experimental until thoroughly tested).

This PR also cleans up some module-level import side effects (e.g., running `distributed.get_worker()` upon importing `pyrosetta.distributed.cluster`) which were subtly affecting the PyRosetta documentation builds with `sphinx`.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b230e431d8ef`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.