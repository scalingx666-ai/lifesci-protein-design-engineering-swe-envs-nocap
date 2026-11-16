# Issue

**Title:** Apply Rule of Zero to core signal event hierarchies

## Description

## Summary

Each of the simple polymorphic signal-event classes in `core/conformation/signals/` and `core/pose/signals/` had a hand-written copy constructor, copy assignment, and empty destructor that all just did what the compiler-generated versions would do. Removing the boilerplate brings them in line with the Rule of Zero while preserving the polymorphic base destructors as `= default`.

Touched:

- `core/conformation/signals/`: `GeneralEvent`, `ConnectionEvent`, `IdentityEvent`, `XYZEvent`
- `core/pose/signals/`: `GeneralEvent`, `DestructionEvent`, `ConformationEvent`, `EnergyEvent`

`LengthEvent` is intentionally left alone — its copy constructor and copy assignment invoke `check_consistency()` in debug builds, so they have real semantic content beyond member-wise copy.

Net diff: 8 files changed, 4 insertions(+), 188 deletions(-).

## Test plan

- [x] `mode=debug` build passes (clean, `-Werror`)
- [x] `mode=release bin` build passes
- [ ] CI green

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b8e64ecdc033`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.