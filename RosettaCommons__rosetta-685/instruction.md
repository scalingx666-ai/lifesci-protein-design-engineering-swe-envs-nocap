# Issue

**Title:** Modernize copy prevention in core factory singletons and non-copyable classes

## Description

## Summary

- **SilentStructFactory, ConstraintFactory, SequenceFactory**: all inherit from `utility::SingletonBase`, which already `= delete`s copy construction and copy assignment. Removes the now-redundant private-but-unimplemented declarations from each derived class (pre-C++11 idiom).
- **OrbitalsStatistics, AtomTreeDiff**: had private-but-undefined copy constructors (pre-C++11 idiom); replaced with explicit `= delete` on both copy constructor and copy assignment operator.

No behaviour change; build-only verification is sufficient for pure copy-prevention modernisation.

## Test plan
- [x] Debug build passes (`scons.py -j16 mode=debug`)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b8e64ecdc033`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.