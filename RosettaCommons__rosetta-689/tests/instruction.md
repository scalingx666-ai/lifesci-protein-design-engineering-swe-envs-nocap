# Issue

**Title:** Apply Rule of Zero to disulfide energy neighbor iterators

## Description

## Summary

- Remove user-declared trivial (defaulted) destructors from the six disulfide neighbor iterator classes; the implicit virtual destructor inherited from `ResidueNeighborIterator` / `ResidueNeighborConstIterator` does the same job.
- Modernize the old-style C++03 copy-assignment prevention (private undefined declaration) to a public `= delete` for the derived-type `operator=`. Behavior is unchanged: derived-derived assignment is still forbidden so all assignments flow through the polymorphic `operator=(BaseType const &)` override that downcasts and copies all members.

Touched: `CentroidDisulfideNeighborIterator`/`ConstIterator`, `DisulfideMatchingNeighborIterator`/`ConstIterator`, `DisulfResNeighbIterator`/`ConstIterator`.

## Test plan

- [x] Full release build (`scons mode=release bin`) passes with no errors
- [x] No external call sites reference the removed destructors or the deleted operator=

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b8e64ecdc033`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.