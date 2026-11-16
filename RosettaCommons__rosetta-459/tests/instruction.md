# Issue

**Title:** Add support for filtering results in PyRosettaCluster

## Description

Similar to the idea of filters in RosettaScripts, it can be useful to filter a result mid-trajectory in `PyRosettaCluster` (rather than solely post hoc filtering data). This PR supports optionally filtering results between user-defined PyRosetta protocols in `PyRosettaCluster`. 

Currently, when a user-defined PyRosetta protocol returns or yields `None` (e.g., a `PackedPose` was discarded in a protocol), `PyRosettaCluster` converts it to an empty `PackedPose` object that gets passed to the next protocol. This can result in needless additional overhead in `dask` because the empty `PackedPose`s still get distributed to all downstream PyRosetta protocols -- even if the protocols don't operate on empty input `PackedPose`s. The user usually has to catch these at the top of their protocols (e.g., with `if packed_pose.pose.empty(): return None`). This PR supports a new `PyRosettaCluster` instance attribute, `filter_results`, that dynamically filters out any empty `PackedPose`s that arrive back to the head node. If enabled, this is used for filtering out decoys mid-trajectory between user-provided PyRosetta protocols. 

Because this changes `PyRosettaCluster` behavior, if there are any users who happen to rely on these empty `PackedPose`s to generate results in downstream protocols (which are user-defined, so there may be scripts in the wild that depend on it not filtering), then decoy IDs in downstream protocols would be changed if enabled. However, because users ought to be implementing an identical PyRosetta version when reproducing results, and because of a somewhat slower billiard spawn method (introduced in #487) that would add additional compute overhead if kept `False` by default, this PR sets the default to `True` (with a temporary courtesy notice `FutureWarning` if not set), and users can explicitly set it to `False` for backward compatibility.

Additionally, this PR ensures that the `filter_results` instance attribute cannot be overridden during reproduction. Hence, this is a new feature that can optionally be enabled for reduced overhead.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `50a63df69605`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.