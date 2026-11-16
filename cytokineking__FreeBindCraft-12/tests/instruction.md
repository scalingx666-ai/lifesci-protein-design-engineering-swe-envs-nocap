# Issue

**Title:** Aligned classification of glycine to apolar to match Rosetta

## Description

Changed classification of glycine to apolar to match Rosetta definition in pr_alternative_utils.py for surface_hydrophobicity as well as interface_hydrophobicity. Also adjusted pyrosetta_utils.py to include glycine as hydrophobic in interface_hydrophobicity (not an active filter, but to maintain consistency). 

Re-arranged pr_alternative_utils.py for readability.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `44e11486f242`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.