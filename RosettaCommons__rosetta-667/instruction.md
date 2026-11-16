# Issue

**Title:** Refactor ozstream raw pointers to unique_ptr; modernize orstream copy ops

## Description

## Summary

- Replace three raw owning pointers in `ozstream` (`char_buffer_p_`, `zip_stream_p_`, `mpi_stream_p_`) with `std::unique_ptr`, eliminating manual `delete`/`nullptr` pairs in `open()`, `open_append()`, `open_append_if_existed()`, `close()`, and the buffer helpers
- Modernize the private-undefined copy constructor/assignment in `orstream` to explicit `= delete`, consistent with the approach used in `irstream`/`izstream` (PR #664)

## Test plan

- [x] Full debug build passes (`python3 ./scons.py -j16 mode=debug`)
- [ ] No behavior change expected — ownership semantics are identical; `close()` still runs `zflush_finalize()` and `mpi_ostream::close()/clear()` before releasing the pointers

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `8556abcecef0`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.