# Issue

**Title:** Refactor izstream raw zip_stream_p_ pointer to unique_ptr

## Description

## Summary
- `izstream` held a raw owning `zlib_stream::zip_istream*` managed by a custom destructor, with copy/assignment only blocked by inherited `private` declarations in `irstream` — no explicit policy on `izstream` itself
- Replace `zip_stream_p_` with `std::unique_ptr<zlib_stream::zip_istream>`, eliminating the manual `delete` in the destructor
- Explicitly `= delete` copy constructor and copy assignment directly on `izstream`
- Update all allocation/reset sites in `izstream.hh` and `izstream.cc` to use `unique_ptr::reset()`

## Test plan
- [x] `utility/io` builds cleanly in debug mode
- [x] Full debug build passes with no errors

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `f9c5b025aa62`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.