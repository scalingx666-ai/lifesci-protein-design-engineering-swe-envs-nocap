# Issue

**Title:** Fix import of load_ligand.py

## Description

Fix issues with `import pyrosetta.toolbox.load_ligand` -- This won't fix things for people who use the functions in question, but it will allow for use of the other functions in this module.

Thanks to @decortja for bringing this to my attention.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `695b0f21aed8`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.