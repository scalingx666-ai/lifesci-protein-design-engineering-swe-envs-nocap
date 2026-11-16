# Issue

**Title:** Add saving of the beautification diff to the test server.

## Description

From a comment by @JackMaguire in #197, we should be able to save the full diff of the beautification test output, and present it on the benchmark server in the `Test files:  file-tree-view 」「 file-list-view 」` entries.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `3366cf78a3df`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.