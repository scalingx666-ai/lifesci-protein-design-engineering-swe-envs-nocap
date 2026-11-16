# Issue

**Title:** Fix memory leak in SimulateMPI: use unique_ptr for simulation_ ownership

## Description

## Summary

- `initialize_simulation()` unconditionally assigned `new SimulateMPIData*` without deleting any previous allocation, leaking memory on re-initialization
- `simulation_` was a raw owning static pointer with no cleanup path
- Fixed by switching `simulation_` from `SimulateMPIData*` to `std::unique_ptr<SimulateMPIData>`, so `reset()` frees the old object on re-initialization and the static is properly cleaned up at program exit

## Test plan

- [ ] Build `utility` library in debug mode: `cd source && ./scons.py -j16 mode=debug utility`
- [ ] Run utility unit tests: `cd source && python test/run.py -j16 --mode=debug`

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `068bd8532215`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.