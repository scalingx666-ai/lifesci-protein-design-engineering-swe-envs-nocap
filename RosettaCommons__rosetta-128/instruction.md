# Issue

**Title:** Generate random keys for scattered data, and optionally delay results in PyRosettaCluster

## Description

The aim of this PR is to fix two issues in PyRosettaCluster:

1. Add the `client.scatter(hash=False)` option, so that random keys are assigned to data rather than hashed from the scattered data. If workers are processing two identical tasks on the same machine, then this can lead to a `KeyError`, so `hash=False` prevents this. This should be the case anyway since user-provided PyRosetta protocols may be deterministic or stochastic (and we're already submitting them to the client as [impure functions](https://distributed.dask.org/en/stable/client.html#pure-functions)), and repeated tasks may have their own destiny (depending on choreography of the simulation) and be processed at different times. Overall, this update doesn't affect user-facing functionality, but it prevents errors from occurring if two tasks happen to stem from identical data, so it's considered a bug fix.
2. Add a new PyRosettaCluster attribute, `max_delay_time`. A dask scheduler may get overwhelmed with large task graphs, many workers, and quickly executing tasks. Tasks run on workers asynchronously from the dask scheduler checking up on them, so a task may complete before the scheduler knows it is being processed, leading to a race condition. To address this scenario, users now have a safety throttle to delay returning results from a worker to the dask scheduler, allowing the scheduler to orchestrate the simulation properly. In practice, this option only takes effect in cases when the user returns a result immediately (e.g., `def protocol(packed_pose, **kwargs): return None`). The worker will delay the result for a default of 3 seconds (at least 1 second is recommended). In cases where user-provided PyRosetta protocols are running computation that takes longer than 3 seconds, then this update has no effect. Users can set this option to 0 seconds for existing functionality, but may encounter occasional`CommClosedError`s depending on a number of factors (e.g., existing network traffic and latency, task graph size, number of workers, compute resource utilization, etc.).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `1a5e0485141f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.