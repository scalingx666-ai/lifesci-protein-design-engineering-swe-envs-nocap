# Issue

**Title:** Prevent openMM from linking separate chains in concatenated structure

## Description

When target PDB has more than one chain, colabdesign concatenates al the target chains into Chain A. OpenMM inappropriately treats the chains as continuous and links them, distorting the structures.. This also occurs when there are discontinuous segments in a singe chain. To resolve this issue, PDB structures are now de-concatenated prior to openMM relax functions, so each chain (including discontinuous segments of a single chain) is provided with a distinct chain ID (starting with C). After relaxation, the PDB file is then re-concatenated for downstream scoring and analysis.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `aaa4b76f97bd`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.