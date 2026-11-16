# Issue

**Title:** BindSweeper fixes

## Description

A few fixes to BindSweeper, including:
- fixing incorrect default values for the results config that were preventing result files from being written
- removal of dummy paths when performing a dry run. Now the output directory and config paths are actually validated
- BindSweeper profiles were being appended each time, and this could result in profiles being generated with the same name. I have changed the behaviour so that a new bindsweeper.config file is generated each time a sweep is launched.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `2fac1aa64aef`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.