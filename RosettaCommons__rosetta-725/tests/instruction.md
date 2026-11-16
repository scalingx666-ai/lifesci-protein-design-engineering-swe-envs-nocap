# Issue

**Title:** Apply Rule of Zero to protocols/ classes with redundant destructors

## Description

## Summary

Applies the Rule of Zero to a set of `protocols/` classes that declared a redundant special member — a destructor equivalent to the implicitly-generated one — and lets the compiler generate it instead. Each removed destructor was either `= default` or an empty `{}` body, and in every case the class holds only **non-owning** raw pointers (back-references it does not allocate or free), so no destructor logic is lost and ownership semantics are unchanged.

Classes updated:

- `antibody/clusters/CDRClusterSet` — drop `= default` dtor (`ab_info_` is a non-owning back-pointer).
- `hbnet/NetworkState` — drop empty `~NetworkState(){}` (non-owning `HBondEdge const *`).
- `jobdist/AtomTreeDiffJobDistributor` — drop `= default` dtor (non-owning `Pose const * last_ref_pose_`).
- `nmr/pcs/AtomGridPoint` — drop empty dtor (non-owning `Residue const *`).
- `noesy_assign/FloatingResonance` — drop `= default` dtor (non-owning `ResonanceList const *`).
- `noesy_assign/PeakAssignment` — drop `= default` dtor (non-owning `CrossPeak *`).
- `peptide_deriver/PeptideDeriverBasicStreamOutputter` — drop `= default` dtor **and** the user-declared copy constructor, which performed a plain memberwise copy (`out_p_`, `prefix_`) identical to the implicit one. `out_p_` is a non-owning `orstream *`.

For the classes deriving from a base with a virtual destructor (`override` dtors), the implicitly-generated destructor is virtual and inherited, so polymorphic destruction is unchanged. Removing these destructors also re-enables implicit move operations (previously suppressed), which is harmless for these value/back-pointer members. A couple of now-moot "auto-generated destructor" doc comments were removed alongside their declarations.

## Note on diff size

This is a small diff (32 deletions). The `protocols/` `rule_of_zero` scan surfaced ~26 candidates, but the large majority are **not** safe to refactor — they own their pointers (e.g. MPI buffers and arrays allocated/freed in the destructor, like `triangleIterator`'s `delete gradPtr`), are singletons/factories that legitimately prevent copying, are observers whose destructors detach from a subject, or have intentionally partial/custom copy constructors. After vetting each candidate's `.hh` and `.cc`, the seven classes here are the subset whose destructor is provably redundant and whose pointers are non-owning. The risk profiles of the excluded candidates genuinely diverge, so they are intentionally left out rather than bundled.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `9567e54bafda`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.