# Issue

**Title:** Removing variadic keyword parameters from function signatures in PyRosettaCluster

## Description

This PR fixes a minor UX issue in `PyRosettaCluster`, improving API usability.

In `PyRosettaCluster`, each user-defined task dictionary is unpacked at the call site of a few internal API functions, and the keyword arguments are then collected by variadic keyword parameters during execution. However, if a user-defined task dictionary contains keys that match the positional-or-keyword parameters in these function signatures, a `TypeError` is raised due to the argument receiving multiple values. These parameters currently include: `protocol`, `packed_pose`, `datetime_format`, `norm_task_options`, `ignore_errors`, `protocols_key`, `decoy_ids`, and `serializer`. As a remedy, herein we remove the variadic keyword parameters from the function signatures, and add relevant unit tests. Since the internal API was already passing positional arguments, redefining the variadic keyword parameters to positional-or-keyword parameters now prevents name clashes when also defined as keyword arguments by the user.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `597b55d6600c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.