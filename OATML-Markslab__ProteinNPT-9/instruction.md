# Issue

**Title:** Fast weights and pipeline cleanup

## Description

Main changes: 
- Added weights (modified msa_data accordingly)
- included latest EVE refactorings of MSA_data (e.g. `preprocess_msa` is its own function)
- called embeddings.py's function directly instead of the script (didn't do this for the other steps in the pipeline, but e.g. train would be easy to do)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `8444884a7a15`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.