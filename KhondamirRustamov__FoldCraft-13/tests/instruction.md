# Issue

**Title:** Perf: keep the fold-conditioned cmap in memory instead of reloading it per model

## Description

## What

The fold-conditioned cmap and its mask are built once and saved to
`fold_cond_cmap.npy` / `fold_cond_cmap_mask.npy`, then **re-read from disk via
`np.load` on every model build** — once per design trajectory *and* once per
MPNN sample. That's `mpnn_samples × num_designs` reloads per run of a
`(target_len + binder_len)²` float64 array, even though the arrays never change.

This keeps them in memory after the `np.save` and hands each model a fresh copy.

## Why it's safe (bit-identical)

- The arrays are constant for the whole run (written once, never modified).
- The loss callback (`cmap_loss_binder`) only **reads** `opt['cond_cmap']` /
  `opt['cond_cmap_mask']` — it never mutates them.
- Each model still gets a fresh `.copy()`, preserving the previous
  per-iteration isolation that `np.load` provided.
- The `.npy` files are still written to disk for inspection/reproducibility.

No change to the design objective, RNG, or any AF2/MPNN forward pass — purely
eliminates redundant disk I/O.

## Stacking

Built on top of the cmap-module extraction (it reuses the in-memory `fc_cmap`
and `binarize_cmap`). Merge order: **#9 → #10 → #11 → this**. Until those merge,
the diff against `main` shows their commits too; the only commit unique to this
PR is the single FoldCraft.py change.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `9a7a0941b11e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.