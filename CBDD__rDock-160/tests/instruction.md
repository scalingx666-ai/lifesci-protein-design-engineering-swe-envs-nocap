# Issue

**Title:** Update supported distros

## Description

* add centos stream 10
* add debian 13
* remove debian 10
* fix typo in ubuntu 22 and 24
* update checkout action in all the workflows

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `3e631b52c0f3`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.