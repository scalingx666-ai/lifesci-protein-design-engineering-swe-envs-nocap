# Issue

**Title:** protenix: expose per-chain-pair ipTM

## Description

### Summary

Surface the **per-chain-pair ipTM matrix** (`pair_chains_iptm`) for Protenix, matching what Boltz-2 already emits. It's already computed inside `ConfidenceHead._chain_ptm_iptm` — the pairwise ipTM is built to derive the per-chain `chain_iptm` and then discarded. This returns it, puts it in the confidence dict, and writes it to the protenix `results.json` rows.

### Why

Global `iptm` is a single scalar; downstream selectivity-aware scoring (and hallucination) needs the **per-chain-pair** ipTM, e.g. `chain_pair_iptm[binder][target]`, to score one interface in a multi-chain complex. Boltz-2 already exposes this as `pair_chains_iptm` (`compute_ptms` → `out_dict["pair_chains_iptm"]`); this brings Protenix in line. No new computation — the values already exist, they were just dropped before the return.

### Change

- `ConfidenceHead._chain_ptm_iptm` → also return the nested `pair_chains_iptm` (`{chain_i: {chain_j: ipTM}}`); single-chain / invalid inputs return the extra `None`.
- `ConfidenceHead.confidence` → `out["pair_chains_iptm"] = pair_chains_iptm`.
- protenix `_row` (worker.py) → include `pair_chains_iptm` in `results.json` when present.
- Host-only test (`tests/test_chain_pair_iptm.py`): matrix is symmetric, values in [0,1], and `chain_iptm[c]` equals the mean over that chain's pair entries.

### Validated on hardware (Blackhole p150a, protenix-v2)

Two-chain complex (binder + target), `--diffusion_samples 1`:

```
results.json: pair_chains_iptm: {'0': {'1': 0.947925}, '1': {'0': 0.947925}}
global iptm:  0.947925
```

Symmetric, and for a two-chain complex the matrix entry equals the global `iptm` — i.e. the surfaced values are consistent with the existing scalar. Clean fold, no regression.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `6ef0721e6b05`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.