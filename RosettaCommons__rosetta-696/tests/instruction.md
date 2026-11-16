# Issue

**Title:** Fix incorrect comparison logic in operator< / difference_from

## Description

## Summary

Four independent classes had broken comparison logic that violated either the contract of \`operator<\` (strict weak ordering) or the intended semantics of \`difference_from\`. Each is a separate, surgical fix bundled into a single PR because they all share the same \"comparison-operator misuse\" theme.

* **\`core/chemical/sdf/mol_util.cc\`** — \`BondData::operator<\` was \`(lower < other.lower) || (upper < other.upper)\`. This is not a strict weak ordering: for example, \`(5, 1)\` and \`(3, 7)\` each compare less than the other, breaking antisymmetry. \`BondData\` is held in \`std::set<BondData>\` (see \`parse_bond_type_data\`), so the broken ordering directly affects the set. Replaced with a proper lexicographic compare via \`std::tie\`.

* **\`core/scoring/motif/motif_hash_stuff.cc\`** — \`ResPairMotif::operator<\` returned \`0 < memcmp(...)\`, which is true exactly when \`memcmp\` says *this > other*; the comparison was inverted. Switched to \`memcmp(...) < 0\` (matches \`MotifHit::operator<\` in the same file).

* **\`core/io/StructFileReaderOptions.cc\`** — \`operator<\` used \`==\` instead of \`!=\` for the short-circuit \`return false\` lines. The intended pattern (used correctly by the parent \`StructFileRepOptions::operator<\` and by \`ImportPoseOptions::operator<\`) is \`if a < b return true; if a != b return false; // fall through\`. The \`==\` form returns false when members are equal, short-circuiting before the rest of the members are compared, *and* falls through when one member is greater. Fixed every \`==\` to \`!=\` and added the missing \`!=\` follow-up after \`glycam_pdb_format_\`.

* **\`core/chemical/gasteiger/GasteigerAtomTypeData.cc\`** — \`difference_from\` short-circuited \`return Other\` when \`charge_ == OTHER.charge_ || element_type_ != ... || ...\`. The first clause is inverted: the function should return \`Other\` when fundamental properties *differ*, not when they match. The bug also made the lone-pair / s-orbital / p-orbital classification logic below this check unreachable for any two types with equal charge. Fixed \`==\` to \`!=\`.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `d10f3af574b6`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.