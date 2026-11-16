# Issue

**Title:** Fix for the maintenance.documentation test

## Description

With the new open repository, direct committing to the repos don't really work anymore.

Instead, we can have the test simply test for changes, and provide a patch file that can be applied to the documentation repository.

This PR also updates the documentation repo to pull in all the changes made through the wiki interface since the last update.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `74f819c4dd58`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.