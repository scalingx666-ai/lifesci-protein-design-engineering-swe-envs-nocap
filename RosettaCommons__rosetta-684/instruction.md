# Issue

**Title:** Default MetricValue special members

## Description

## Summary
- default no-op MetricValueBase destructor
- default MetricValue default and copy constructors

## Testing
- python3 ./scons.py -j16 mode=debug
- python3 test/run.py -j16 --mode=debug MetricValueIOTest
- python3 ./scons.py -j16 mode=release bin

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b8e64ecdc033`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.