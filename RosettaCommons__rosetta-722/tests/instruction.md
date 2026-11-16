# Issue

**Title:** Fix Slab geometry thickness/steepness argument order in MembraneInfo

## Description

## Summary

. `Slab`'s constructor is `Slab(core::Real steepness, core::Real thickness)`, but two `MembraneInfo` constructors in `core/conformation/membrane/MembraneInfo.cc` called it with the arguments in the wrong order.

**(1) Plain 6-arg constructor** passed `Slab(thickness, steepness)`, so the `Slab` geometry stored thickness and steepness swapped. The `fa_2012` energy methods (`FaMPEnv`/`FaMPSolv` → `mpframework_smooth_fa_2012`) read thickness/steepness off the geometry, so this silently changed scores (e.g. with `thickness=15.6, steepness=10`, the IMM1 transition midpoint became 10 Å instead of 15.6 Å with a far steeper transition). Now passes `Slab(steepness, thickness)`.

**(2) Implicit-lipid constructor** passed `Slab(thickness, steepness)`, where the bare `thickness` token was *not* the member `thickness_` (just set from `implicit_lipids_->water_thickness()`) but the unscoped `MEM::thickness` enum (`== 1`) from `MembraneParams.hh` — both live in `core::conformation::membrane`. The call was therefore effectively `Slab(1, steepness)`: the lipid water thickness was dropped and `1` landed in the steepness slot. Now passes `Slab(steepness, thickness_)`, matching the sibling lipid + `MP_GEOMETRY_TRANSITION` constructor's `SLAB` case. This path is currently latent (these constructors set `implicit_lipids_`, so `use_franklin()` is true and the geometry thickness/steepness aren't read), but it was still wrong.

All other `Slab` call sites in the file already use the correct `(steepness, thickness)` / `(steepness, thickness_)` order, so these two lines are the complete set of violations.

Diagnosis and reproduction by @vuoanh in #719.

## Note on diff size

This is a 2-line, single-file change — below the usual bundling threshold. There is no sibling buggy pattern to bundle: every other `Slab` construction in the file is already correct, so this is the complete and self-contained fix for the reported issue.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `f8a8eab4344a`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.