# Issue

**Title:** Simplify installation instructions

## Description

I found the current installation process challenging because it combines many calls to conda and pip. I simplified it by creating a conda .yml file to install almost all of the dependencies. A few still are pip installed. One needs to be pip installed outside the .yml file because of the `--no-dependencies` argument. ProteinDT itself is also pip installed after the conda environment is created. Open Fold is currently commented out because the pinned version fails in a system without a GPU.

I created a GitHub Actions workflow for testing that the new installation process works and that ProteinDT can be imported. Python 3.7 gave some errors, but I was eventually able to resolve those by switching to an older version of pip. We could use this workflow to test if the installation also works with newer Python.

I have not been able to test that ProteinDT actually runs in this environment or that the GPU support works. That should be tested before merging. If this looks okay, I can also update the readme before merging.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `06a018923cc9`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.