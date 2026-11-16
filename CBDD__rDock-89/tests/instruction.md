# Issue

**Title:** Issue #82/replace libpopt in rbcavity

## Description

* decouple argument parser setup, parsing, entry-point and rbcavity procedure
* create `RbtArgParser.h` file with common and helper functions regarding parser setup
* create `RbtPlatformCompatibility.h` file to get rid of the "unwieldy" `#ifdef ...` in the middle of functions. in this case, for `ios_base::openmode` for input and output.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `97a05d6156fd`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.