# Issue

**Title:** Add IAM::set_interface(string) for PyRosetta.

## Description

BindCraft (and others) use the InterfaceAnalyzerMover.set_interface() function with a string. This was removed during the multi-character chain designation update.

This PR puts it back for PyRosetta.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `fa5ce209894d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.