# Issue

**Title:** Refactor pimpl pointers in XMLSchemaComplexTypeGenerator and XMLValidator to unique_ptr

## Description

## Summary

- Replace raw `class Impl*` pimpl members with `std::unique_ptr<class Impl>` in `XMLSchemaComplexTypeGenerator` and `XMLValidator`
- Destructors changed from `{ delete pimpl_; }` to `= default`, letting the compiler generate correct cleanup
- Added `#include <memory>` to both headers

## Motivation

Both classes already correctly suppressed copy/move with `= delete`, so there was no double-free risk. This refactor makes ownership explicit in the type system and eliminates manual `delete` calls, following the Rule of Zero pattern for pimpl idiom.

## Testing

Full debug build succeeds. No behavior change — pure ownership semantics refactor.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `068bd8532215`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.