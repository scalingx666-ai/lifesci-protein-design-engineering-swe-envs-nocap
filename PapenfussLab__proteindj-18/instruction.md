# Issue

**Title:** Fix: Upgrades RFdiffusion and AF2 containers to CUDA 12.4 and 12.9 for improved hardware compatibility

## Description

Most of our containers had CUDA 12 except for RFDiffusion and AF2-InitialGuess. I have rewritten these container definitions and package dependencies to use CUDA 12 which improves compatibility with recent GPU hardware (as requested in #12). 

I have also removed the strict python version call in the RFdiffusion module (python3.10).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c26b1f5f8243`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.