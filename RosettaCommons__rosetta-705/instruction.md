# Issue

**Title:** Apply Rule of Zero to utility/keys container family (corrected)

## Description

## Summary

Re-applies the Rule-of-Zero refactor that was originally landed as #691 and then reverted in #703, fixing the bug that caused the revert. Supersedes #704 (the revert-revert), which still ships the same broken state and will fail `cat=test` on `test/utility/keys/ClassKeyMap.cxxtest.hh`.

### What was broken in #691 / #704

The original commit dropped the explicit `= default` default constructors from `ClassKeyMap`, `ClassKeyVector`, and `KeyVector` on the assumption that they would be synthesized implicitly. They are not. Each of those classes has a user-declared (non-default) constructor — the templated iterator-range constructor on `ClassKeyMap`, and the size / uniform-value / iterator-range constructors on `ClassKeyVector` and `KeyVector`. By the C++ rules, any user-declared constructor suppresses the implicit default. That broke `Map m;` in the unit test (`test/utility/keys/ClassKeyMap.cxxtest.hh`):

```
./test/utility/keys/ClassKeyMap.cxxtest.hh:32:7: error: no matching constructor
for initialization of 'Map' (aka 'ClassKeyMap<int, int, float>')
                Map m;
                    ^
```

### What this PR does

Same Rule-of-Zero cleanup as #691 — drops the redundant user-declared destructors, copy constructors, and copy-assignment operators on all five classes, so the implicitly defaulted versions (now including move constructor and move assignment) take over — but keeps an explicit `= default` default constructor on `ClassKeyMap`, `ClassKeyVector`, and `KeyVector`. `SmallKeyMap` and `SmallKeyVector` already keep a user-defined default constructor because they need to value-initialize the scalar `Index u_`.

Verified by building `mode=debug cat=test` end-to-end (no compile errors).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b2a0ff32f3ee`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.