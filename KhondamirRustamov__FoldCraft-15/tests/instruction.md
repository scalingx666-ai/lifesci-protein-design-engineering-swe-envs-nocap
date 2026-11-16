# Issue

**Title:** Remove dead calc_ss_percentage / calculate_percentages helpers

## Description

## What

`calc_ss_percentage` and `calculate_percentages` in `biopython_utils.py` are
unreachable:

- `calc_ss_percentage` has **no caller** anywhere in the repo — `FoldCraft.py`,
  `FoldCraft_binder.py`, and the notebooks were all checked.
- `calculate_percentages` is called **only from inside** `calc_ss_percentage`.

This removes both. That orphans the `DSSP` and `collections.defaultdict` imports
(used only by `calc_ss_percentage`), so they go too; I also dropped the
already-unused `Polypeptide` / `PDBIO` / `Select` / `Chain` names from the
`Bio.PDB` import line while I was there.

## Safety

- Both `from biopython_utils import *` consumers (`FoldCraft.py`,
  `test/FoldCraft_binder.py`) reference **none** of the removed names — verified
  by grep — so the star-import re-exports aren't relied on.
- The module still imports cleanly with `set_range`, `hotspot_residues`,
  `target_pdb_rmsd`, `calculate_clash_score`, `validate_design_sequence` intact.
- Drops the now-obsolete `TestCalculatePercentages` tests and the `tests/README`
  bullet. Remaining suite passes.

## Stacking

Builds on the `set_range` fix. Merge order: **#9 → #10 → this**.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `3caa374b8a59`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.