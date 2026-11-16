# Issue

**Title:** Fix correctness bugs: broken operator< and off-by-one residue loops

## Description

## Summary

Two unrelated families of correctness bugs found by an audit pass. Continues the patterns from #696 (broken comparison operators) and #695 (off-by-one residue loops). Diff is small (~45 lines) because each fix is one-liner-scale and no further sibling candidates of either pattern were found in this audit pass.

### Broken `operator<` implementations (violate strict weak ordering)

- **`core/environment/LocalPosition.cc`** — assigned the signed result of `std::string::compare()` to the unsigned alias `Size`, so any `this->label_ < other.label_` case wrapped to a huge value and the `cmp > 0` branch fired. On top of that, the `cmp > 0` branch itself was inverted relative to "less than". Combined effect: for any two `LocalPosition`s with distinct labels, both `a < b` and `b < a` returned `true`. The class is used as the key type of `std::map<LocalPosition, core::Real>` in `CutBiasClaim`, so the broken ordering silently corrupted lookups/insertions in those maps. Replaced with `std::tie` lexicographic compare.

- **`protocols/environment/claims/ClaimStrength.cc`** — every comparison operand was reversed, so `operator<(*this, other)` returned `true` exactly when `*this` was lexicographically greater than `other` in `(subtype_, subprio_)`. The operator was fully inverted relative to its name and to the (correctly written) `operator==` in the same file. Replaced with `std::tie` lexicographic compare.

### Off-by-one loops over 1-indexed residue / `vector1` ranges

- **`protocols/simple_moves/ExplicitWaterMover.cc`** `get_water_recovery`: four loops using `i < pose.total_residue()` (and `i < vec.size()` on `utility::vector1<>`, which is 1-indexed). The last residue and last vector element were skipped, so water-recovery counts and the per-water coordinate lists silently dropped the final entry whenever the last residue was a water or every iteration was needed.

- **`protocols/ligand_docking/GALigandDock/GALigandDock.cc`** `calculate_free_ligand_score`: the loop adding `CoordinateConstraint`s to anchor every atom used `ires < pose->total_residue()`, so the last residue's atoms were not constrained during free-ligand minimization.

- **`protocols/membrane_benchmark/MembraneEnergyLandscapeSampler.cc`** `count_res` / `count_diff`: both helpers walked `ii < pose.total_residue()`, so a matching last residue was never counted and a differing last residue's mismatch was never reported.

All fixes follow the same `i <= N` (or `std::tie`) idioms used in the previously-merged patches.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `dc594471462f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.