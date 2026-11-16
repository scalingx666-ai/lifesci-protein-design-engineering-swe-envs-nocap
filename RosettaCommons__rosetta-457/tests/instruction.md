# Issue

**Title:** Add cartesian option to RosettaLigand FinalMinimizer.

## Description

Alex was interested in potentially having Cartesian minimization in RosettaLigand docking (to possibly better optimize a interaction controlled by a non-torsion.

This is straightforward to add.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `931f7d9046b1`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.