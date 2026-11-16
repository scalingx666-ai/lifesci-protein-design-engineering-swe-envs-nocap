# Issue

**Title:** Add the deterministic_flag option to the ProteinMPNNProbabilitiesMetric SimpleMetric

## Description

This PR adds the `deterministic_flag` option to the `ProteinMPNNProbabilitiesMetric` PerResidueProbabilitiesMetric. After some evidence provided in #429, the `ProteinMPNNMover` already has the `deterministic_flag` option set to `false` by default, and therefore due to Torch randomness the `ProteinMPNNProbabilitiesMetric` can return non-deterministic PSSM tables, even with the `-run:constant_seed 1` Rosetta option enabled. This PR just adds some control of the `ProteinMPNNMover` from the `ProteinMPNNProbabilitiesMetric` to enable deterministic mode when calculating per-residue ProteinMPNN probabilities.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `fce88fc4bd52`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.