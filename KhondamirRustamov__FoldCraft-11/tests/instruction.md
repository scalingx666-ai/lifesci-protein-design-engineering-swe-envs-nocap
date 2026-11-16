# Issue

**Title:** Extract the fold-conditioned cmap into a tested pure module

## Description

The fold-conditioned contact map is the deterministic artifact the design loss conditions on. It was built inline in `FoldCraft.py main()`, **duplicated** across the VHH and standard paths, and untestable without a GPU.

- **`cmap_utils.py`**: pure-numpy `assemble_fold_conditioned_cmap()` (+ `apply_binder_mask`, `binarize_cmap`), shared by both paths. `binder_mask` indexes residue R at `R-1` (consistent with the hotspot indexing) and raises on out-of-range residues.
- **`FoldCraft.py`**: both inline blocks replaced with calls; the GPU/model code is untouched (keeps the VHH-CLI fix and `os.makedirs`).
- **`tests/test_cmap_utils.py`**: differential test vs an independent re-implementation (standard / default-hotspot / masked / VHH-like), plus adversarial `binder_mask` tests (terminal + out-of-range).

```
35 passed
```

**Stacked on #10** (the `binder_mask` adversarial tests assume the inclusive `set_range`). Merge order: #9 → #10 → this.

🤖 Generated with [Claude Code](https://claude.com/claude-code)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `9a7a0941b11e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.