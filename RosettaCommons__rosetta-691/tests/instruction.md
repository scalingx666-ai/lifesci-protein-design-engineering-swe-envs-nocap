# Issue

**Title:** Apply Rule of Zero to utility/keys container family

## Description

## Summary

Applies Rule of Zero to five sibling key-container templates in `source/src/utility/keys/`:

- `ClassKeyMap`
- `ClassKeyVector`
- `KeyVector`
- `SmallKeyMap`
- `SmallKeyVector`

Each held only standard-container value-type members (a `Vector`, plus an `IndexMap` and a scalar `Index u_` in the `Small*` variants). Their user-declared destructors (empty body or `= default`), copy constructors, and copy-assignment operators were byte-for-byte equivalent to the implicit defaults the compiler would synthesize, so they were redundant.

Removing them lets the implicitly defaulted special-member-functions take over and, as a side effect, restores the implicit move constructor and move assignment that were previously suppressed by the user-declared copy operations. The explicit default constructors on `SmallKeyMap` and `SmallKeyVector` are kept because they value-initialize the scalar `Index u_`, which an implicit default would leave indeterminate. `ClassKeyMap` / `ClassKeyVector` / `KeyVector` previously had `= default` default constructors that are now also implicit.

Net: 5 files changed, 140 lines removed, 0 added.

## Test plan

- [x] Debug build: `cd source && python3 ./scons.py -j16 mode=debug` — completed with exit 0, no errors.
- [ ] Standard test suite (label `90 standard tests` requested).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b8e64ecdc033`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.