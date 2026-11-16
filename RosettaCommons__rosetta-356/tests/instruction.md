# Issue

**Title:** Better debugging on CIF file read in.

## Description

Issue #355 indicates that errors in mmCIF parsing are not well handled. This commit attempts to fix that in two ways.

The first is to add a 'from extension' fallback if the from-contents parser doesn't like it. We assume that if you're naming it xyz.pdb, you want it parsed as a PDB.

The second is to print the diagnostics report for mmCIF files to the tracer -- this is what's being used to autodetect mmCIF format, so better transparency of what the failure issue is would be warranted.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `029c6a159b89`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.