# Issue

**Title:** Fix off-by-one bugs in 1-indexed loops over pose residues

## Description

## Summary

Replace \`i != end\` with \`i <= end\` (or \`i != size()\` with \`i <= size()\`) in several loops that iterate over 1-indexed residue ranges. With the old condition, the last residue (index == end) was silently skipped.

* \`core/pack/task/PackerTask_.cc\`: pymol-style debug selection output omitted the last residue.
* \`protocols/forge/components/BDR.cc\`:
  - the full-atom check used to gate \`switch_to_residue_type_set\` skipped the last residue, so a non-full-atom last residue could bypass conversion and later cause sidechain-restore mismatch (the precise failure the surrounding comment warns about).
  - the loop building the \`RestrictResidueToRepacking\` operation skipped the last residue, so a C-terminal residue outside \`new_positions\` was designed instead of repack-only.
* \`protocols/fldsgn/BluePrintBDR.cc\`: same full-atom check bug as in \`BDR.cc\`.
* \`protocols/denovo_design/components/StructureData.cc\`: \`compute_cutpoints\` skipped the last residue, dropping a \`CUTPOINT_LOWER\` variant on the C-terminal residue.
* \`protocols/enzdes/BackboneSampler.cc\`: the user-requested number of backbone-Monte-Carlo trials was off by one (e.g. \`bb_moves_=1000\` actually ran 999 trials, in contradiction with the \"Running N trials...\" log line printed just above).

These are all instances of the same pattern: a 1-indexed loop using \`!=\` against a one-past-the-last bound that was actually meant to be the last valid index. Each loop body uses the index directly to access pose data, so the fix is to switch the comparison to \`<=\`.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `d10f3af574b6`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.