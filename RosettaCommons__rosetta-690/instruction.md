# Issue

**Title:** Apply Rule of Zero to BitSet and BitVector

## Description

## Summary
- `utility::BitSet<B>` and `utility::BitVector<B>` each had only a single value-type member (`std::set<B>` and `std::vector<bool>` respectively), no user-declared copy/move/assignment operators, and an explicit empty destructor.
- The empty `~BitSet() {}` / `~BitVector() {}` did nothing the compiler-generated destructor wouldn't have done, and they suppressed the implicit move constructor and move assignment.
- Remove the empty destructors so both class templates follow the Rule of Zero and gain implicit move support.

## Test plan
- [x] `python3 ./scons.py -j16 mode=release bin` builds cleanly.
- [ ] CI: 90 standard tests.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b8e64ecdc033`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.