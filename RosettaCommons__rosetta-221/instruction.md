# Issue

**Title:** Bugfix for RosettaCM and branched glycans

## Description

RosettaCM, with a branched glycan ligand, was segfaulting. In 2 locations, the "previous residue" of the given polymer entity was selected by choosing the position prior to the one in question, which is not appropriate for a branched glycan. The fix was to set a conditional for carbohydrates that uses conformation::carbohydrates::find_seqpos_of_saccharides_parent_residue to find the previous residue.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `d1d7693ff7c1`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.