# Issue

**Title:** Apply Rule of Zero to remaining LREnergyContainer iterator subclasses

## Description

## Summary

Extends the disulfide-iterator pattern from PR #689 to the four remaining `ResidueNeighbor{,Const}Iterator` subclasses in `core/scoring/`:

- `DenseNeighbor{,Const}Iterator` (`DenseEnergyContainer.{hh,cc}`)
- `OneToAllNeighbor{,Const}Iterator` (`OneToAllEnergyContainer.{hh,cc}`)
- `PolymerBondedNeighbor{,Const}Iterator` (`PolymerBondedEnergyContainer.{hh,cc}`)
- `CstResNeighb{,Const}Iterator` (`constraints/ConstraintEnergyContainer.{hh,cc}`)

For each class:

- Remove the empty out-of-line destructor (`~X() override;` decl in `.hh` plus `X::~X() = default;` in `.cc`) — Rule of Zero suffices since the base destructor is already virtual.
- Replace the pre-C++11 private undefined derived-derived copy-assignment (`X & operator = (X const & );`) with an explicit `= delete` and a short comment explaining that all assignment must funnel through the polymorphic `operator = ( ResidueNeighbor{,Const}Iterator const & )` so derived members are downcast and copied correctly.

Together with PR #689 this exhausts the `LREnergyContainer` iterator family — every concrete container's iterator pair now follows the same idiom.

The bundle is smaller than the usual minimum (~77 lines) because no further sibling work exists in this hierarchy.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `708ccac48829`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.