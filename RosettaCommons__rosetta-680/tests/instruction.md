# Issue

**Title:** Fix key map range indexing

## Description

## Summary
- fix ClassKeyMap and SmallKeyMap range constructor/assignment loops to visit the final 1-indexed element
- add regression coverage for range construction and assignment preserving the final key

## Testing
- python3 ./scons.py -j16 mode=debug cat=test
- python3 test/run.py -j16 --mode=debug KeyMapTests SmallKeyMapTests
- python3 ./scons.py -j16 mode=release bin

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `cf532f89b6bb`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.