# Issue

**Title:** Refactor VariantKey and VariantOption raw pointers to unique_ptr

## Description

## Summary
- `VariantKey<K>` and `VariantOption<O>` each held a raw owning pointer with a manually-defined copy-constructor, copy-assignment, and destructor, but were missing move-constructor and move-assignment (Rule of Five violation)
- Replace `K* key_p_` / `O* option_p_` with `std::unique_ptr<K>` / `std::unique_ptr<O>`
- Add `= default` move-constructor and move-assignment, giving correct move semantics for free
- Remove the manual destructors; `unique_ptr` handles cleanup
- Preserve deep-copy semantics in copy-constructor and copy-assignment using `unique_ptr::reset()` with the existing `clone()` pattern
- Update pointer-conversion operators in `VariantOption` to use `.get()`

## Test plan
- [x] `utility/options/keys/OptionKeyList.cc` and `protocols/jd3/standard/StandardJobQueen.cc` compile cleanly (syntax check)
- [x] Full debug build passes with no errors

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `068bd8532215`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.