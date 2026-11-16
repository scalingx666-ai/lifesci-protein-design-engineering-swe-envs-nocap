# Issue

**Title:** FakeRotLib and minor tweaks to m2pp.py

## Description

This pull request is to introduce the FakeRotLib script formally into the rosetta codebase.  The script takes in a NCAA sdf, generates conformers using RDKit, parameterizes it using molfile_to_params_polymer.py, and optionally creates a rotlib file describing the rotamer distributions of the chi torsions.

A few small tweaks were also made to molfile_to_params_polymer.py to ensure that the backbone hydrogen is always described using a proper torsion and not an improper torsion and to ensure guanadinium nitrogens are properly typed.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `029c6a159b89`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.