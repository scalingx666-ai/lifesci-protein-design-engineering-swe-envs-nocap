# Issue

**Title:** change numpy.alltrue to alltrue.all to improve compatibility with Numpy-2.0

## Description

related Slack conversations:
https://rosettacommons.slack.com/archives/C2MLV37T9/p1727811520389129
https://rosettacommons.slack.com/archives/C2MLV37T9/p1742318420039749

related Numpy documentation: https://numpy.org/doc/1.26/reference/generated/numpy.all.html#numpy.all (note that while docs have not mentioned it, the `alltrue` method have remained in the package until v2.0 was introduced.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `05ba3ce142c6`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.