# Issue

**Title:** FoldCraft.py robustness & performance fixes

## Description

A handful of small, independent fixes to `FoldCraft.py` (plus the shared `biopython_utils.py` / `cmap_utils.py` and the experimental `test/FoldCraft_binder.py`) from running the pipeline internally. Each is its own commit — happy to split if you'd prefer.

1. **VHH framework path** — `np.load('framework/vhh.npy')` was CWD-relative, so a `--vhh` run launched from anywhere but the repo root failed with `FileNotFoundError`. Anchored to the script directory.
2. **Robust binder-template sequence** — built with `seq1()` over every PDB residue including HETATM waters/ions, which inflated the length (a spurious "gaps in residue numbering" abort on valid crystallographic templates) and could mis-read modified residues like MSE. Now taken from the model's own `_wt_aatype` after `prep_inputs` (HETATM-free, MODRES-correct), with the gap guard counting polymer residues via `is_aa`.
3. **Build ProteinMPNN once** — `mk_mpnn_model` (which reloads the weights checkpoint and rebuilds its jitted fns) was constructed inside the per-trajectory loop although its args are constant; hoisted out. Caveat handled in the follow-up commit: `clear_mem()` (called per trajectory) deletes the hoisted model's device-resident RNG key, so `set_seed(None)` is re-applied after each `clear_mem()` (the params are host-side numpy and survive). Distribution-preserving — a fresh key per trajectory, as before.
4. **`--sample` exact count** — `while passed <= success_target` overshot by one, and the inner MPNN-batch loop kept saving every passing sample past the target (up to `mpnn_samples - 1` extra). Now produces exactly `--target_success` via `< success_target` plus a small `iter_until_target` budget helper.
5. **Crash-safe results.csv** — written once at the very end, so an OOM/preemption mid-run lost the whole metrics table. Now streamed to `results.csv.partial` and atomically promoted to `results.csv` only on completion.
6. **Build the AF2-ptm prediction model once per trajectory** — it was rebuilt inside the `for num, seq` loop for every MPNN sample though only `set_seq` differs, re-running template featurization + a fresh XLA compile each time. Build once before the loop, reuse across the batch. Validated **bit-identical** on an A100 (plddt/i_ptm/i_pae/cmap_loss match to 6 decimals, predicted coordinates exactly equal — no state leak) with a **2.87× speedup** on the predict loop.
7. **Empty-`binder_hotspots` off-by-one (cmap)** — when `--binder_hotspots` is omitted ("all binder residues"), the default used a 0-based `range(0, binder_len)` while the explicit path uses 1-based `set_range`, shifting the interface block one row: a spurious contact on the last target residue and the binder C-terminal residue dropped from conditioning. Fixed to 1-based; the differential test's reference implementation copied the same bug (so it passed tautologically) — fixed that too and added explicit tests pinning the correct behavior.

All GPU-free logic is unit-tested (`tests/test_biopython_utils.py`, `tests/test_cmap_utils.py`); the suite passes. Off current `main`.

🤖 Generated with [Claude Code](https://claude.com/claude-code)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `3caa374b8a59`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.