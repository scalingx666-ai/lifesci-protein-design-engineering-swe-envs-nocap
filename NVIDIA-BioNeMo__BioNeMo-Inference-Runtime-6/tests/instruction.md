# Issue

**Title:** fix(openfold3): match AF3 pTM/ipTM reduction and confidence bin centers

## Description

## Description

Two deviations in `bionemo_ir/pipeline/models/openfold3/postprocessor.py` (line numbers at `bbf5f9ec`) from the AlphaFold 3 definitions (Abramson et al., *Nature* 2024, SI §5.9.1, eqs. 17–18) and from upstream OpenFold3 (`openfold3/core/metrics/confidence.py`):

1. **pTM / ipTM reduction.** `_tm_score_from_pae_logits` (lines 262–282) returns the mean of the expected TM term over all token pairs (pTM) or over all inter-chain pairs (ipTM). AF3 and upstream `compute_ptm` take, for each aligned token *i*, the mean over scored tokens *j* and then the **maximum over *i*** (restricted to tokens with a valid frame). The pair mean is a lower bound of that quantity, so 0.1.0 reports systematically low pTM/ipTM — not comparable with the usual ipTM thresholds or with `0.8·ipTM + 0.2·pTM` — while the PAE matrix itself is correct. The Boltz-2, Protenix and OpenFold2 paths in this repository already use row-mean-then-max with bin midpoints; the OpenFold3 post-processor was the exception.
2. **Bin centers.** The TM term (line 270), `_compute_pae` (298), `_compute_plddt` (209) and `_select_best_sample` (188) weight bin probabilities with `torch.linspace(lo, hi, n_bins)` end points instead of bin midpoints (upstream `get_bin_centers`: 0.25 … 31.75 Å for PAE, 0.01 … 0.99 for pLDDT). For PAE and pLDDT this is an exact increasing affine map, so the pipeline's own best-sample selection (mean-pLDDT argmax) is unchanged by this PR.

### Changes (one PR, two commits, bisectable)

1. `fix(openfold3): match AF3 pTM/ipTM reduction (row mean, max over aligned tokens)` — reduction fix, TM term at bin midpoints, optional `has_frame` argument. At the call site the mask is derived from the existing `is_atomized` feature (polymer tokens are frame-eligible; ligand atoms / ions are scored as *j* but not aligned on). This equals upstream's mask for polymer residues and single-atom ligands and is a lower bound for multi-atom ligands (upstream's `get_token_frame_atoms` is not ported here). If no token is eligible the score is `nan` (→ `None` in `get_scores()`), like the existing single-chain ipTM case. `get_scores()` gains one optional key, `ptm_frame_mask`, recording which convention produced the values — happy to drop it if you prefer the scores schema unchanged.
2. `fix(openfold3): use bin midpoints for PAE and pLDDT expectations` — `_compute_pae`, `_compute_plddt`, `_select_best_sample`. Shifts every PAE entry by ≤ 0.25 Å and every pLDDT / B-factor / ModelCIF QA value by ≤ 1; kept separate from commit 1 because it touches many outputs but changes no ranking.

Conversions for existing 0.1.0 outputs: `PAE_new = 63/64 · PAE_0.1.0 + 0.25 Å`; `pLDDT_new = 49/50 · pLDDT_0.1.0 + 1` (0–100 scale); `max_pae` ceiling 32.0 → 31.75 Å; pTM / ipTM have no closed form — recompute from the PAE logits.

### Example (public PDB entries)

Same PAE logits through the 0.1.0 and the patched post-processing, next to upstream OpenFold3 run separately on the same inputs (one seed, mean over 5 diffusion samples, so the upstream column agrees within sample-to-sample scatter rather than exactly):

| entry | tokens | pTM 0.1.0 → patched | pTM upstream | ipTM 0.1.0 → patched | ipTM upstream |
|---|---|---|---|---|---|
| 9H1J (two protein chains) | 172 | 0.552 → 0.745 | 0.744 | 0.307 → 0.415 | 0.410 |
| 9RVG (homodimer + 2 citrate) | 410 | 0.636 → 0.831 | 0.834 | 0.586 → 0.812 | 0.818 |

On both entries pLDDT and PAE change only by the affine maps above, and the pipeline's selected sample is the same before and after.

### Tests

`tests/pipeline/models/openfold3/test_openfold3_ptm.py` (new; synthetic inputs): max-over-aligned-tokens for pTM (0.856 vs 0.037 with the pair mean) and ipTM (inter-chain rows only); TM term at bin midpoints with the AF3 `d0` (incl. the N < 19 clip); `has_frame` semantics incl. the all-masked / ligand-only case; `(B, S, N, N, 64)` slicing; PAE = 0.25 + 0.5k Å and pLDDT = 2k + 1 for one-hot bins; best-sample argmax invariance; parity with upstream `compute_ptm`, `probs_to_expected_error` and `compute_plddt` (|Δ| < 1e-5; skipped when the `3rdparty/openfold-3` submodule is not checked out). With both commits: 15 pass, 1 skipped (an env-gated report helper); on current `main` 13 of these fail. `ruff check` and `ruff format --check` are clean. The tests were run on CPU here — please let CI confirm on GPU.

## Type of change

- [x] Bug fix (non-breaking)
- [ ] New feature (non-breaking)
- [ ] Breaking change
- [ ] Refactor
- [ ] Documentation
- [ ] Build / CI

## Checklist

- [x] My commits are signed off (DCO): `git commit -s`
- [x] I have read the Contributing Guidelines
- [ ] For a new feature or breaking change, an issue was filed and approved first — n/a (bug fix)
- [x] I added or updated tests, and they pass locally
- [ ] I updated documentation as needed — n/a

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `bbf5f9ecb553`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.