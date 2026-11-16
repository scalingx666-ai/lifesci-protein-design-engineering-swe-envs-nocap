# Issue

**Title:** Add cooldown time for PyRosettaCluster unit tests

## Description

This quick PR aims to add a `cooldown_time` instance attribute to `PyRosettaCluster` (with a default of 1 second) so loggers can flush after the simulation is complete but before unit test cases exit. This makes more sense than the unit test sleeping after already exiting.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `58d657c59974`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.