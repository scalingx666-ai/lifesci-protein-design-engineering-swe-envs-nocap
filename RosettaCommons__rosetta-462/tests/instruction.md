# Issue

**Title:** Support saving PackedPose objects and arbitrary score data in PyRosettaCluster

## Description

This PR adds support for `PyRosettaCluster` to output decoys in the `.pkl_pose` and `.b64_pose` file formats (introduced in #431), as well as to output scorefiles in pickled `pandas.DataFrame` format (which supports arbitrary datatypes that have been saved in the `PackedPose.scores` dictionary as introduced in #430). 

Currently, `PyRosettaCluster` instance keyword arguments are saved on `REMARK` lines in output PDB files, and in this PR `PyRosettaCluster` instance keyword arguments are technically cached in the `pyrosetta.rosetta.core.pose.datacache.CacheableDataType.STRING_MAP` datacache, which results in a `REMARK` line if output as a PDB file, yet we save it as a `.pkl_pose` or `.b64_pose` file. Therefore, this PR supports reproducing a decoy from a `.pkl_pose` or `.b64_pose` file that has been output by `PyRosettaCluster`. The benefit of saving decoys in the `.pkl_pose` and `.b64_pose` formats over PDB format is that arbitrary python types are still cached in the `.pkl_pose` and `.b64_pose` files and do not require JSON-encoding (since JSON-encoding can't serialize arbitrary data types). Additionally, `.pkl_pose` and `.b64_pose` files save the exact atomic coordinates of the pose, and therefore it can be demonstrated that a decoy can be recursively reproduced _ad infinitum_ through a set of custom PyRosetta protocols with exact atomic coordinates (note that `PyRosettaCluster` has already been doing this, but because the PDB file format rounds the atomic coordinates it was not possible to explicitly show this).

Furthermore, this PR supports saving `PyRosettaCluster` scorefiles as pickled `pandas.DataFrame` files, which also can store arbitrary python types from the `PackedPose.scores` dictionary. Decoys can also be reproduced from a scorefile in pickled `pandas.DataFrame` format (which supports many additional compression types, including `.gz`, `.bz2`, `.xz`, `.tar`, `.tar.gz`, `.tar.xz`, and more). Please note that `pandas` is already a dependency of `dask`, and is therefore already a dependency of `pyrosetta.distributed`, so new third-party dependencies are not being added with this PR.  

This PR also adds a slight guardrail when attempting to reproduce decoys with a PyRosetta version that is not exactly identical to the version used to produce the original decoy. Currently, `PyRosettaCluster` just gives a warning if the PyRosetta versions are not identical between the build that created the original decoy and the current build being implemented for reproduction. In this PR, if there's a mismatch we raise an error instead, and the user must manually input an empty string to the `pyrosetta_build` keyword argument to bypass PyRosetta build validation. This is similar logic as the conda environment validation that is already in place.

Note: this PR can be merged after merging #430, since there is some overlapping code.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c389ea27189e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.