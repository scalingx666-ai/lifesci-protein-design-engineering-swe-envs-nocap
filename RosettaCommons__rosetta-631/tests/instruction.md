# Issue

**Title:** Enable task dictionary updates between PyRosetta protocols in PyRosettaCluster

## Description

This quick PR adds an attribute validator in the `PyRosettaCluster` class for user-defined task dictionaries, verifying that they don't contain disallowed or reserved keys/values for proper `PyRosettaCluster` runtime behavior.

Additionally, this PR adds:

- Validation for task dictionary updates between user-defined PyRosetta protocols.
- A minor update to the `norm_task_options` attribute functionality to only normalize task options for the first PyRosetta protocol. Previously, we presumed users would not update task options at all, so re-normalizing task options for each protocol was reasonable. This now allows updating task options between protocols while supporting output decoy reproducibility.
- Unit tests for decoy reproducibility while updating task dictionaries between user-defined PyRosetta protocols.
- A patch to the `InitFileSigner` class for HMAC-SHA256 verification of PyRosetta initialization files. Previously, we used the pickled `Pose` bytestring for HMAC-SHA256 verification; however, for some `Pose` objects the ordering of internal C++ containers may differ upon reconstruction from the original bytestring, sometimes failing roundtrip HMAC-SHA256 verification. Herein, we implement a new `PackedPoseHasher` class to instead produce a SHA256 hash digest of what we may call the scientific state of the `Pose` object, which includes the `Pose` geometry (i.e., residue numbers, residue names, atom numbers, atom names, and atom coordinate components in double precision), serialized `Pose.cache` dictionary keys and values, and raw `Pose` comments keys and values. The scientific state of the `Pose` object is what matters for decoy reproducibility, not differences in memory layout from serialization/deserialization roundtrips.

To be merged after #630 for incremental versioning.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `775949edbbbd`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.