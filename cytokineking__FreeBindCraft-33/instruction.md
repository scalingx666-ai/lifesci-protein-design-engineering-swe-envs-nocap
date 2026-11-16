# Issue

**Title:** Experiment with opt-in BF16 attention epilogue fusion on MetalJAX

## Description

Evoformer attention repeatedly launches separate BF16 masking/bias/clipping and softmax operations on Metal. Add an opt-in native forward/backward epilogue fusion as a patch on the frozen `0.11.9+fbc1` MetalJAX source. It retains the attention matrix products, derivative matrix products, intermediate BF16 rounding and frozen MLX reduction order.

The experiment is disabled unless `METALJAX_EVO_ATTN=1`. Admission is bounded to BF16 `[B,H,Q,K]`, heads 4/8, Q >= 16 and 65 <= K <= 512, with row/element-count checks and structural matching. Unsupported graphs retain the existing path. FreeBindCraft carries the source patch, an isolated maintainer builder, optional diagnostic profiling patch and artifact/validation manifests. No new public wheel or release is published, and the production installer continues to select the existing frozen fbc1 wheel. Shared scientific and CUDA code are unchanged.

Validation on an M5 Max with 128 GiB unified memory, JAX/jaxlib 0.11.2, BF16/high precision, rematerialization and one recycle:

- Reversed fresh-process series, four processes per arm: warmed sequence-gradient median 1.2187 -> 1.1069 s (9.2% less time); forward 0.3895 -> 0.3414 s (12.3%). First calls were slightly slower. Per-call native dispatch counts confirm actual execution of the candidate path.
- Unchanged fixed-work harness, two fresh processes per arm in ABBA order: 140-iteration design median 221.21 -> 197.24 s (10.8% less time). All 140 printed iteration records, confidence checkpoints, actual generated design PDB bytes, raw validation PDB bytes and complete work counts match. Physical-footprint high-water marks were about 9.5–9.8 GB baseline versus 9.0 GB candidate, without observed swap growth.
- All 18 fixed-input full-model fields, three warmed-forward fields and three first-forward recycle-state fields match byte for byte at 146 and 185 residues.
- 76 regression tests passed. Focused differential coverage includes 24 cases, two input variants, supported and unsupported patterns, every input derivative, clip ties, layouts, rematerialized scans and nonfinite/fully masked semantics.

The full earlier timing series, including a candidate slowdown concurrent with macOS background services, is retained in the validation record. Rejected small-width arithmetic, an early whole-tape fallback and observer-affected profiling experiments are documented. Diagnostic fence times are dependency-region observations, not exclusive production operator costs. Thermal and desktop GPU state remain uncontrolled; these are bounded numerical/performance checks on one Mac, not accepted-binder throughput or CUDA equivalence.

Implementation, reproduction commands, limits and evidence: `installation/metaljax/attention/README.md`, `validation.json` and `build-manifest.json`. Raw arrays, HLO, binaries and continuous activity logs remain in the ignored local run directory.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `137d98dddc61`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.