# Issue

**Title:** Quick update: change which HKDF function is used by HMAC-SHA256 verification machinery in PyRosettaCluster

## Description

This quick patch has virtually no effect on security or runtime functionality, it just updates which extract-and-expand hash-based message authentication code (HMAC)-based key derivation function (HKDF) is used for pseudorandom key generation in task dictionary HMAC-SHA256 verification, reflecting original intention.

To be merged after #648 for incremental versioning.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ea526b88894f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.