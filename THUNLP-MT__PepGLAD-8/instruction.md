# Issue

**Title:** Update for dependencies, peptide restype, and support for openmm8.2.0

## Description

Hi, 

I've made a couple rather small changes to make this code runnable on my machine in 2025/04. 

1. In order to run the repository on H100s, the pytorch version in `env.yml` is too old. I've updated it to torch==2.6.0. So far, the model inference is runnable without issues on the given example. 
2. For my personal usage, I run into cases where the peptide chain doesn't have standard residue types or atom annotations, and this caused issues with the `detect_pocket.py` script. I don't think this is entirely necessary for all use cases, but I included them for those in the same situation as me.
3. Since I've updated the pytorch version, I've also updated all other dependencies, and this includes openmm (8.0.0 ->8.2.0). Run into some small issues related to tolerance specification (see `evaluation/dG/openmm_relaxer.py`, specifically `ForceFieldMinimizer.__init__(...)`). It was specified with an energy metric, but that caused some issue from openmm8.2.0, and updating it to a force metric worked. All three metrics are listed as global variables as before. 

PepGLAD is a cool and useful tool; I hope you can consider some of these changes. 

Best.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `2474da017cc6`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.