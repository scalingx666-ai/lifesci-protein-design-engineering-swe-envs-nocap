# Issue

**Title:** Novel-ligands

## Description

Upodate allows the inclusion of small molecular ligands that are not contained in the PDB. This update mainly handles the correct protonation of those ligands with reduce, which works based on a look up.
We assume that this unknown ligand is stored in chain "L" and has name "UNL" for simplicity.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `49668c27afb0`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.