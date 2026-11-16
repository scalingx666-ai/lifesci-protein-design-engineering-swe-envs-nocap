# Issue

**Title:** Use unchunked Metal execution by default

## Description

ColabDesign uses sub-batches of four above 384 total target + binder residues. Disable this chunking by default on Apple Silicon to avoid severe Metal overhead, preparing both forward and gradient functions consistently. Complete AlphaFold recycles were about **3× faster** in the tested 406-residue case, at the cost of higher memory use.

| Test job | Unchunked peak memory | Original policy |
| --- | ---: | ---: |
| Ubiquitin 76 + binder 70 = 146 residues | ~6.9 GiB | ~6.9 GiB |
| CA2 256 + binder 150 = 406 residues | ~27.5 GiB | ~18 GiB |

Set `FREEBINDCRAFT_METAL_CHUNKING=1` to restore four-residue sub-batches above 384 residues when memory is limited. Memory safeguards remain enabled; CPU/CUDA behavior is unchanged.

Validation: 38 CPU tests passed. The restored chunked GPU path matched all 33 original-control output arrays and used 18.0 GiB. Full-workflow benchmarks remain the draft gate.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `056ff605371c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.