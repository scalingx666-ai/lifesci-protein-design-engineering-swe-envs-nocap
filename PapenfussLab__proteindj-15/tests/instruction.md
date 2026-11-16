# Issue

**Title:** Bindcraft fixes

## Description

This update fixes handling of Nextflow null values for hotspots and omit_AAs by BindCraft preparation script. It also provides some fixes to Seqera schema files for different modes.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b20c24b69252`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.