# Issue

**Title:** Explicitly delete copy constructor and assignment in Timer

## Description

## Summary
`Timer::~Timer()` calls `Report()`, logging the elapsed time to a tracer or stdout. With no explicit copy policy, a copied `Timer` would produce a second `Report()` on its own destruction — silently logging a duplicate timing entry for the same measured interval.

`tag_` and `t_` are non-owning pointers (no `delete` in the destructor), so there is no memory-safety risk. The issue is purely semantic, but it is a real hazard for callers who pass or store `Timer` by value.

Explicitly `= delete` the copy constructor and copy assignment to make the non-copyable intent clear.

## Test plan
- [x] `basic/gpu` builds cleanly in debug mode
- [x] Full debug build passes with no errors

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `f9c5b025aa62`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.