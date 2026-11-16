# Issue

**Title:** Refactor Svm_rosetta raw pointer to unique_ptr with custom deleter

## Description

## Summary
- `Svm_rosetta` held a raw `svm_model*` and defined a custom destructor but no copy constructor/assignment, making the compiler-generated copies unsafe (double-free on destruction)
- Replace `svm_model*` with `std::unique_ptr<svm_model, SvmModelDeleter>` where `SvmModelDeleter` calls `svm_free_model_content` + `free`
- Explicitly delete copy constructor and copy assignment on `Svm_rosetta`
- Remove the unnecessary `= default` destructor from `Svm_node_rosetta` (no resources to manage)

## Test plan
- [x] `utility/libsvm` compiles cleanly in debug mode
- [x] Full debug build passes with no errors or warnings

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `068bd8532215`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.