# Issue

**Title:** Modernize copy prevention in utility/signals classes to use = delete

## Description

## Summary

- Replace old-style private-and-undefined copy constructor/assignment in five `utility/signals` classes with explicit `= delete`
- Affected classes: `SignalHub`, `BufferedSignalHub`, `PausableSignalHub`, `TokenHub`, `LinkUnit`
- Also explicitly deletes the `LinkUnit` default constructor (was previously also private-undefined)
- Pure mechanical modernization — no behavioral change

## Motivation

The pre-C++11 idiom of declaring copy ops private with no definition produces a linker error on misuse rather than a clear compile-time diagnostic. `= delete` gives an immediate, descriptive compiler error at the call site and is the modern idiomatic approach.

## Test plan

- [x] `python3 ./scons.py -j16 mode=debug` passes (headers-only change; no behavioral difference)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `8556abcecef0`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.