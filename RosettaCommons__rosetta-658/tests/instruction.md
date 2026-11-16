# Issue

**Title:** Improving `Pose.cache` dictionary getter and setter performance

## Description

This PR aims to improve the performance of `Pose.cache` dictionary data accessors. Several code pathways run with O(N^2) (quadratic time complexity) behavior, and new functionally equivalent fast data accessor methods are introduced to run with O(N) (linear time complexity) behavior:

- `Pose.cache.fast_items()`
- `Pose.cache.fast_values()`
- `Pose.cache.metrics.fast_items()`
- `Pose.cache.metrics.fast_values()`
- `Pose.cache.metrics.real.fast_items()`
- `Pose.cache.metrics.string.fast_values()`
- `Pose.cache.metrics.composite_real.fast_items()`
- `Pose.cache.metrics.composite_real.fast_values()`
- `Pose.cache.metrics.composite_string.fast_items()`
- `Pose.cache.metrics.composite_string.fast_values()`
- `Pose.cache.metrics.per_residue_real.fast_items()`
- `Pose.cache.metrics.per_residue_real.fast_values()`
- `Pose.cache.metrics.per_residue_string.fast_items()`
- `Pose.cache.metrics.per_residue_string.fast_values()`
- `Pose.cache.metrics.per_residue_probabilities.fast_items()`
- `Pose.cache.metrics.per_residue_probabilities.fast_values()`
- `Pose.cache.extra.fast_items()`
- `Pose.cache.extra.fast_values()`
- `Pose.cache.extra.real.fast_items()`
- `Pose.cache.extra.real.fast_values()`
- `Pose.cache.extra.string.fast_items()`
- `Pose.cache.extra.string.fast_values()`
- `Pose.cache.energies.fast_items()`
- `Pose.cache.energies.fast_values()`

Users must update their API calls to take advantage of these upgrades: `dict(pose.cache)` -> `dict(pose.cache.fast_items())`, etc. These improvements are only really noticable when there are hundreds to thousands of scores cached in the `Pose.cache` dictionary. The basis for the performance improvement is the following:
- `dict(pose.cache)` relies on `__iter__` (returns `pose.cache.all`) + `__getitem__(key)` (returns `maybe_decode(pose.cache.all[key])`), where it materializes the full scores dictionary for each key (O(N^2)).
- Instead, `dict(pose.cache.fast_items())` relies on simply `for k, v in pose.cache.all.items(); yield k, maybe_decode(v)`, so the full scores dictionary is materialized once for all keys (O(N)).
- It's also worth noting that the deprecated `Pose.scores` dictionary (note `scores` not `cache`) has always performed with quadratic time complexity (O(N^2)), and does not contain `Pose.scores.fast_items()` or `Pose.scores.fast_values()` methods. 

This PR also makes the `Pose.cache.all_scores` property run with O(N) behavior, and removes an unnecessary argument from a private method: `self._has_sm_data(pose)` -> `self._has_sm_data()`.

Additionally, this PR provides two new fast setter methods for mappables (avoiding the relatively slow `Pose.cache.metrics` cleanup after each item is set with `__setitem__`, and instead only performing one cleanup at the end):
- `Pose.cache.metrics.real.set_mappable()`
- `Pose.cache.metrics.string.set_mappable()`

Micro-updates to the `PyRosettaCluster` interface are made to take advantage of these performance improvements.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `91e8dceb1419`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.