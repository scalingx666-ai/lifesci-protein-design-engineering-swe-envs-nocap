# Issue

**Title:** Unit test fix: pyrosetta.tests.distributed.test_smoke.SmokeTestDistributed.test_packed_pose_io

## Description

This quick PR fixes the `pyrosetta.tests.distributed.test_smoke.SmokeTestDistributed.test_packed_pose_io` unit test. It was referencing obsolete filename extensions, which herein are updated to `.pkl_pose` and `.b64_pose`. The `.init` filename extension is also added for completeness. For coordinate recovery, I also updated the number of decimal places tested to `sys.float_info.dig` (i.e., `15` rather than the arbitrarily high value of `32`), which is a more reasonable limit reflecting the maximum number of decimal digits that can be faithfully represented in a Python `float`.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `612b6ef9e9d4`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.