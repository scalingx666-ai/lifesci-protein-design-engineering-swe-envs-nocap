# Issue

**Title:** ipSAE scoring

## Description

This pull request introduces support for Boltz-2 iPSAE scoring for binder design modes in the pipeline, along with related updates to the workflow and documentation. The main changes include adding the new `BoltziPSAE` process, updating output handling in `RunBoltz`, and modifying the workflow to integrate iPSAE calculations. There are also minor documentation and configuration updates.

**Boltz-2 iPSAE scoring integration:**

* Added the `BoltziPSAE` process to `modules/boltz.nf`, which computes iPSAE and IPAE scores from Boltz-2 predictions using PDB and NPZ files, and outputs results in CSV and JSONL formats.
* Updated the `RunBoltz` process to emit NPZ files required for iPSAE scoring, and modified output file handling to include NPZ files alongside PDB and JSON files.

**Workflow updates:**

* Modified `main.nf` to include the new `BoltziPSAE` process, rebatch NPZ outputs, and conditionally run iPSAE scoring for binder design modes. 

**Documentation and configuration:**

* Updated anchor links and section names for citations in `README.md` for consistency. 
* Added parameters to `nextflow.config` for binder_denovo testing, including options for skipping folding and specifying input/output directories.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `93774b129119`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.