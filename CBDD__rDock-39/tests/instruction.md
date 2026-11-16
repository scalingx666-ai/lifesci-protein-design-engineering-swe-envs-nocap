# Issue

**Title:** Issue 38/fix for clang support

## Description

This PR will fix a long standing issue of undefined behavior because of the initialization of a reference with an object that would be already deconstructed by the time it would be used.

It never caused any problem, as the reference would never be used in this particular case, but it is treated as an error by clang.

In order to avoid this, the proposed solution is to create an object in the RBT namespace, and always use this object. The value is never modified, and it was always empty, so there's no problem with sharing the object.

Also, adds new configurations for the CI pipeline to check rDock is able to be compiled with clang and pass the current tests

()

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `6b34dcebd032`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.