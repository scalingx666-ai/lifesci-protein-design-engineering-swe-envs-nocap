# Issue

**Title:** Fix memory leaks in Inline_File_Provider

## Description

## Summary

- `clear_input_files()` and `remove_input_file()` were leaking `std::stringstream` objects by calling `vector::clear()`/`erase()` without deleting the raw pointers
- `get_ostream()` also leaked a `new std::stringstream` on every call
- Fixed by switching `input_files` and `output_files` members from `std::vector<std::pair<std::string, std::stringstream*>>` to `std::vector<std::pair<std::string, std::unique_ptr<std::stringstream>>>`, so the vector destructor and `erase()` handle cleanup automatically

## Test plan

- [ ] Build `utility` library in debug mode: `cd source && ./scons.py -j16 mode=debug utility`
- [ ] Run utility unit tests: `cd source && python test/run.py -j16 --mode=debug`

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `068bd8532215`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.