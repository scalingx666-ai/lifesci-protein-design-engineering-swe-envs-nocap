# Issue

**Title:** Fix off-by-one in 1-indexed loops in docking/membrane/misc (split 4/4 of #707)

## Description

## Summary

One of **four PRs splitting #707** (originally a single 35-file off-by-one batch) into coherent, per-subsystem pieces, so each can be reviewed and its regression-test impact assessed independently. #707 is being closed in favor of these four. The split is deliberate — the individual diffs are below the usual bundling threshold — because the changes are behavior-affecting and the maintainer asked to isolate regression-test impact per subsystem.

**This PR: docking / membrane / antibody / match / loops / misc movers (11 files).**

### Off-by-one in 1-indexed loops (`<` → `<=`)

The last element was silently skipped:

- `docking/DockingEnsemblePrepackProtocol.cc` ×2 — chain identity validation
- `docking/metrics.cc` ×2 — cutpoint scan in `calc_Fnonnat`
- `membrane/MPLipidAccessibility.cc` ×2 — slice iteration
- `antibody/residue_selector/CDRResidueSelector.cc`
- `match/output/UpstreamDownstreamCollisionFilter.cc`
- `loops/util.cc` ×3 — non-protein-chunk and per-loop accounting
- `cartesian/md.cc` ×2 — per-atom state save / derivative check
- `cutoutdomain/CutOutDomain.cc` — `find_nearest_res`
- `enzdes/EnzRepackMinimize.cc` — movable-residue collection for backrub
- `moves/PyMOLMover.cc` — `relevant_residues` mask init (last entry left default `false` despite the intent of "all relevant")

### Comment-only

- `membrane/util.cc` — documents **why** a nearby loop intentionally stays `< chains.size()` (the trailing MEM virtual chain must be excluded). No behavior change.

## Regression tests

This PR carries **most of #707's results-affecting changes**: `enzdes` and `inverse_rotamer_remodel` (`enzdes/EnzRepackMinimize.cc`), `ligand_dock_cholesterol` (`docking/`), `mp_f19_relax` and `mpil_load_implicit_lipids` (`membrane/MPLipidAccessibility.cc`). These output changes are intentional but results-affecting and warrant scientific sign-off before merge.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `8ebeca78061b`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.