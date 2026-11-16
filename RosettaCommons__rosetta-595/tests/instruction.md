# Issue

**Title:** Preserve Pose bitwise representation when accessing Pose.cache SimpleMetrics data

## Description

This PR aims to make a minor change to the `Pose.cache` infrastructure. 

Currently, accessing SimpleMetrics data in the `Pose.cache` dictionary via the `get_sm_data` method lazily initializes a `SimpleMetricData` entry in the `Pose.data` cache. If a `Pose` does not already contain SimpleMetrics data, then merely accessing this getter method bitwise mutates the `Pose` (albeit harmlessly, since it only instantiates an empty container). However, in principle, a getter ought not to modify the underlying state or binary representation of the `Pose`.

Herein, we instead conditionally check for `Pose.data().has(CacheableDataType.SIMPLE_METRIC_DATA)`, performing the same test at the PyRosetta layer that the `get_sm_data` method performs at the C++ interface. If `False`, then we return an empty dictionary without lazily initializing a `SimpleMetricData` entry in the `Pose.data` cache. This minor update enables deterministic behavior when instantiating `PackedPose` objects, during which the `Pose.cache` dictionary is accessed to define the `PackedPose.scores` attribute. By preventing lazy initialization of the SimpleMetrics data cache, the new behavior is consistent regardless of whether input `Pose` objects contain SimpleMetrics data. Unit tests are also added herein to ensure consistency of the underlying bitwise representations of `Pose` objects with and without SimpleMetrics data.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `612b6ef9e9d4`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.