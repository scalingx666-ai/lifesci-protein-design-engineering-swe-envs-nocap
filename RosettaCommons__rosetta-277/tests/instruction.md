# Issue

**Title:** Add cmath includes

## Description

`std::abs(Real)` needs the real-valued version of std::abs() function, which is found in the cmath header. (Versus the integer-valued versions, which can also be found in the cstdlib header.)

We were missing those imports in a few locations, which was causing my compiler to complain.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `4e95d564dc8b`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.