# Issue

**Title:** Add missing cmath for cos(...) usage

## Description

Hi there,

Over at https://github.com/bioconda/bioconda-recipes/ we had a linking issue while updating our rDock package to version `0.22`.
Our build linked `libRbt` via `RbtDihedralSF.o` with the mangled `_ZGVbN2v_cos` symbol which is available from `libvecm` from glibc >=2.22 .
To create compatible binaries, we link with older glibc versions and as such our build failed due to the missing library/symbol.
Adding the `#include <cmath>` gives us the desired linking with the `(GLIBC_2.2.5) cos` symbol.

Cheers,
Marcel
cc @simonbray

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `7f88b9fa8b05`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.