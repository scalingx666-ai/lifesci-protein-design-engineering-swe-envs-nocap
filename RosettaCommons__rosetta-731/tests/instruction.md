# Issue

**Title:** Fix off-by-one in 1-indexed loops in frag_picker + accounting (split 2/4 of #707)

## Description

## Summary

One of **four PRs splitting #707** (originally a single 35-file off-by-one batch) into coherent, per-subsystem pieces, so each can be reviewed and its regression-test impact assessed independently. #707 is being closed in favor of these four. The split is deliberate — the individual diffs are below the usual bundling threshold — because the changes are behavior-affecting and the maintainer asked to isolate regression-test impact per subsystem.

**This PR: fragment picking + job / metric accounting (9 files).**

Enumeration/accounting loops that use `for ( i = 1; i < container.size(); ++i )` with `i` as a direct 1-indexed accessor, silently skipping the last element. Changed `<` to `<=`.

- `frag_picker/GrabAllCollector.hh` — `clear`
- `frag_picker/VallProvider.cc` — `find_chunk`
- `frag_picker/quota/QuotaCollector.cc` ×5 — per-position pool enumeration
- `frag_picker/scores/AtomBasedConstraintsScore.cc` ×2 — `constrainable_atoms` map fill, per-row state init
- `jd3/JobGenealogist.cc` — `newick_tree`
- `pose_metric_calculators/DecomposeAndReweightEnergiesCalculator.cc`
- `pose_metric_calculators/SurfaceCalculator.cc` — per-residue summary string
- `canonical_sampling/mc_convergence_checks/HierarchicalLevel.cc` — address match count
- `unfolded_state_energy_calculator/UnfoldedStateEnergyCalculatorMover.cc` — protein-residue count

## Regression tests

This PR touches **`unfolded_state_energy_calc`** (via `UnfoldedStateEnergyCalculatorMover.cc` — the trailing residue is now counted). That output change is intentional but results-affecting and warrants scientific sign-off before merge. The remaining files (frag_picker, jd3, metric calculators, `HierarchicalLevel`) are not in the reported changed-test set.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `8ebeca78061b`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.