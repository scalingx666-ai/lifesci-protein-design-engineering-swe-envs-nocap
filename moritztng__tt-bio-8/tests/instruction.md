# Issue

**Title:** exp/bioperf-largeseq: Protenix denoise dispatch optimization

## Description

## Summary
Performance optimization for Protenix denoise tracing on large sequences:
- Cache AdaLN modules + opt-in ttnn trace of denoise (dispatch-bound diffusion)
- Profile diffusion dispatch-bound behavior; sample-batching measured as dead-end
- Documentation + accuracy harness for the denoise-trace win

Experimental branch, parity-validated, ready for review.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `858bc3927530`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.