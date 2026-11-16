# Issue

**Title:** Lanthipeptide and recon multistate design update

## Description

Modifications to lanthionine ring parameters to more accurately model the sidechain torsions and the methyl placement for methyllanthionine. Additionally, the recon multistate design code is updated to allow for design with NCAA by switching from a single letter code sequence stored in a string to a vector1 of strings that store the base name of the residue.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `4e95d564dc8b`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.