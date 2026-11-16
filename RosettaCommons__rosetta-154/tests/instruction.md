# Issue

**Title:** Adding unit test for NotResidueSelector constructors

## Description

My personal code is breaking because passing a NotResidueSelector to a NotResidueSelector calls the copy ctor, not the ctor I intended. Before asking the team what the correct behavior should be, I want to spin up this test to see if I can reproduce the behavior.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c75854a9a1a5`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.