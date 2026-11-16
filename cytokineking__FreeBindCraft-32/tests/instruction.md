# Issue

**Title:** Ship frozen MetalJAX loop fix by default on Apple

## Description

AlphaFold's nested static loops were admitted for compilation but fell back to eager execution when MLX tracing tried to read a device scalar. The frozen FreeBindCraft MetalJAX build carries the proven static trip count into the runtime, retaining the existing compilation limits and recovery behavior.

Make the checksummed `metaljax==0.11.9+fbc1` [release](https://github.com/cytokineking/metaljax/releases/tag/v0.11.9-fbc1) the standard Apple dependency with JAX/jaxlib `0.11.2`. Add `FREEBINDCRAFT_METALJAX=stock` for the pinned upstream troubleshooting option, retain explicit local-wheel overrides, and archive the exact source/build inputs and provenance. CUDA/Linux installation and shared scientific code have no changes.

Validation:
- Independently downloaded and verified all 13 published assets, 74 compiled source hashes, 282 SDK file hashes, wheel RECORD, and the source tag. All seven runtime payload files match the previously reviewed build byte for byte.
- Fresh isolated installation, repeat installation, stock switching/rerun, local override precedence, restoration, corrupt-cache recovery, checksum rejection before package changes, and dependency consistency passed.
- 72 focused Metal/compiler/FreeBindCraft tests passed against the independently installed public wheel; two existing JAX deprecation warnings.
- Independently checked saved 146- and 185-residue outputs: all 18 fields, including sequence gradients, remain bit-identical to stock. Larger-fixture previous states and controlled-memory outputs also match. The retained full-trajectory validation records identical design iterations and unrelaxed predictions.

The performance gain uses more memory: full-workflow peak footprint rose from 5.49 to 8.85 GiB, and the larger forward/gradient probe from 5.14 to 10.43 GiB. Validation is on a 128 GiB M5 Max; smaller physical Macs remain unvalidated. Governor refusals propagate, and its sampled budget is not a strict peak cap. The published payload is unchanged from the reviewed runtime; this review did not repeat a full trajectory or cold native build.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c6f8994bf6de`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.