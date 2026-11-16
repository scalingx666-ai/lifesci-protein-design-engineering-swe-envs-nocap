# Issue

**Title:** move slurm gpu setting insde milton profile

## Description

- Slurm-specific settings in the general config requires overwriting on non-slurm clusters. 
- While this can be overwritten within other institutional profiles, the GPU label will supercede any higher level labels. As a result any higher-level clusterOptions must be duplicated in the GPU config which can be awkward to maintain.
- Moving the slurm GPU settings inside the institutional profile keeps configuration more flexible.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `4b7205893f0f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.