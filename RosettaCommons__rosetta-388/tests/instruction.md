# Issue

**Title:** fix Mac RC release package

## Description

Current Issue: Our Mac binary releases are currently built using the instruction set of the local Intel Xeon CPU (via -march=native), which can result in binaries that fail to run on other x86 CPUs due to unsupported instructions.

Fix: This PR addresses the issue by explicitly setting the instruction set to core2 when compiling on macOS with an x86 CPU, ensuring the resulting binaries are compatible with a wider range of Intel-based Macs.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `695b0f21aed8`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.