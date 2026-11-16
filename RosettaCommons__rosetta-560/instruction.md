# Issue

**Title:** Add uv/conda/mamba unit tests for PyRosettaCluster virtual environment recreation

## Description

This PR is a follow-up to PR #536 to add unit tests for `uv`, `conda`, and `mamba` workflows in the PyRosettaCluster simulation reproduction lifecycle. In particular, this PR changes the `pyrosetta.distributed.cluster.recreate_environment` function to support recreating a `conda`/`mamba` virtual environment in a local prefix directory rather than via a provided environment name to the global `conda`/`mamba` environment context, which mimics the `uv` and `pixi` workflows more closely.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e5ce21e85a29`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.