# Issue

**Title:** Supporting task retries in PyRosettaCluster

## Description

`PyRosettaCluster` supports running tasks on available compute resources; however, often it's more economical to run tasks on preemptible compute resources, such as cloud spot instances or backfill queues. This PR exposes Dask's task retry API via the `PyRosettaCluster.distribute` method, allowing configuration of the number of automatic retries for each submitted task. When the `retries` keyword argument parameter is set, `PyRosettaCluster` will reschedule failed tasks up to the specified number of times if compute resources are reclaimed midway through a protocol.

This PR also adds a logging warning if using the `resources` keyword argument with `dask` version `<2.1.0`.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `5e498f1409c6`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.