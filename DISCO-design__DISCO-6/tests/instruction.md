# Issue

**Title:** Bug/issue 5 rotary emb mismatch

## Description

# Fix: load DISCO checkpoint against newer/downgraded transformers ESM

.

## Summary

The bundled DISCO checkpoint (`DISCO-Design/DISCO` on HF Hub) was saved against an old `transformers` release whose `EsmEmbeddings` only created `position_embeddings` on the absolute-position branch and stored a per-layer rotary `inv_freq` buffer on every attention layer. Both of those have since changed in the upstream ESM rewrite, so `InferenceRunner.load_checkpoint` throws a `RuntimeError` from `load_state_dict(strict=True)`. The first is on extra per-layer rotary `inv_freq` keys, while the second is on a missing shared `rotary_embeddings.inv_freq` / missing `position_embeddings.weight`.

This PR adds two small, narrowly-scoped state-dict remaps that run before `load_state_dict`, so the existing checkpoint loads cleanly on current ESM/DPLM module layouts without weakening `strict=True`.

## Changes

All changes are in `runner/inference.py`:

- **`_remap_esm_rotary_inv_freq`** — when the current model exposes a single shared `…rotary_embeddings.inv_freq` buffer but the checkpoint carries the old per-layer copies, verify all per-layer tensors are identical (they always are by construction) and promote the first one to the shared key, dropping the per-layer entries. No-op on older transformers where the per-layer layout still exists.
- **`_fill_unused_esm_position_embeddings`** — when the current `EsmEmbeddings` instantiates an unused `position_embeddings.weight` (e.g., on transformers 4.50.0) and the checkpoint does not carry that key, fill it from the freshly-initialized current model state. Safe because `position_embedding_type="rotary"` means the weight is never read during the forward pass. No-op on checkpoints that legitimately carry an absolute-position embedding.
- Both remaps are wired into `load_checkpoint` immediately after the existing `module.`-prefix stripping and before the shape-mismatch filter, so the rest of the loading path is unchanged.

## Test plan

- [X] Run `python runner/inference.py experiment=designable input_json_path=input_jsons/unconditional_config.json seeds=[0,1]` end-to-end against the HF-hosted `DISCO-Design/DISCO` checkpoint and confirm it loads and produces outputs.
- [X] Repeat against both the downgraded transformers 4.50.0 (backwards-compat baseline) and the current pinned version to confirm both remaps are no-ops or active as expected.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `8ab83c1fd711`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.