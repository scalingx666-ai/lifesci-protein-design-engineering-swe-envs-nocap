# Issue

**Title:** Modernize copy prevention in factory/Registrant to use = delete

## Description

## Summary

- Replace old-style private-and-undefined copy constructor/assignment in `utility/factory/Registrant` with explicit `= delete`
- Added a one-line comment explaining the non-obvious reason: each constructor calls `ProductFactory::add()`, so a copy would silently re-register the same key

## Test plan

- [x] `python3 ./scons.py -j16 mode=debug` passes (header-only template change; no behavioral difference)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `5c9f6db69d1a`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.