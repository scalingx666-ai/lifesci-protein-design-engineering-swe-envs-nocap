# Issue

**Title:** Enhance pyrosetta and pyrosetta.distributed I/O interfaces

## Description

This PR adds convenience functions (with unit tests) to the `pyrosetta.io` and `pyrosetta.distributed.io` namespaces, in particular promoting buried functions like `pyrosetta.rosetta.core.io.pdb.dump_multimodel_pdb` to the top-level namespace like `pyrosetta.dump_multimodel_pdb`:

Upgrades to the `pyrosetta` and `pyrosetta.io` namespaces:
- Input functions (i.e., files to `Pose` objects):
  - Defines `pose_from_pdbstring`
  - Defines `poses_from_multimodel_pdb`
- Output functions (i.e., `Pose` objects to files):
  - Imports `dump_pdb`
  - Defines `dump_scored_pdb`
  - Imports `dump_cif`
  - Imports `dump_mmtf`
  - Defines `dump_multimodel_pdb`
  - Defines `dump_file`
- Score functions:
  - Imports `create_score_function` in `pyrosetta.io` namespace
  - Imports `get_fa_scorefxn` in `pyrosetta.io` namespace
  - Imports `get_score_function` in `pyrosetta.io` namespace

Upgrades to the `pyrosetta.distribued.io` namespace:
- Input functions (i.e., files to `PackedPose` objects):
  - Refactors `poses_from_files`
  - Defines `pose_from_pdb` (thanks to @ajasja for the suggestion to combine `pose_from_pdb_bz2`/`pose_from_pdb_gz` functions together, and @lyskov for suggesting support for `xz` compression -- this one function now handles all 3 compression formats)
  - Defines `pose_from_base64`
  - Defines `pose_from_pickle`
  - Defines `poses_from_sequences`
  - Defines `poses_from_multimodel_pdb`
- Output functions (i.e., `Pose` or `PackedPose` objects to files):
  - Defines `dump_pdb`
  - Defines `dump_scored_pdb`
  - Defines `dump_cif`
  - Defines `dump_mmtf`
  - Defines `dump_multimodel_pdb`
  - Defines `dump_pdb` (@ajasja @lyskov this one function supports `bz2`, `gzip` and `xz` compression)
  - Defines `dump_base64`
  - Defines `dump_pickle`
  - Defines `dump_file`

- Conversion functions:
  - Defines `pose_result`
  - Defines `PackedPose.clone()` method
  - Defines `to_base64`
  - Defines `to_pickle`

- `ScorePoseTask` instantiation functions (mimicking those in `pyrosetta.*` and `pyrosetta.io.*` namespaces):
  - Defines `create_score_function`
  - Defines `get_fa_scorefxn`
  - Defines `get_score_function`

While methods like `pose.dump_cif` are already handy, these existing methods are promoted to the top-level namespaces mainly to match new functions like `pyrosetta.distributed.io.dump_base64` and `pyrosetta.distributed.io.dump_pickle`.  These are particularly useful portable representations of poses since they save the atomic coordinates with exact machine precision as well as save the `Pose.scores` dictionary to the file. So, for example, when `pyrosetta.distributed.io.pose_from_base64` is used in a round-trip (save/load) procedure, the resulting pose is loaded exactly how it was when it was saved (contrary to `dump_pdb` and `pose_from_file` which round atomic coordinates and do not load saved scores into the pose).

This PR also fixes a `DeprecationWarning` in `bindings/energies.py`

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `a0cefad01b39`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.