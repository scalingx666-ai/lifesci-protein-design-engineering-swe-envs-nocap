# Issue

**Title:** Support task scheduling priorities in PyRosettaCluster

## Description

This PR adds support for finer control of task execution orchestration in PyRosettaCluster by exposing Dask's work priority API controlling Dask schedulers. There are two major task execution patterns that the user may wish to follow when setting up a PyRosettaCluster simulation:

1. _Breadth-first task execution:_ Currently, tasks are always run generally following a first-in, first-out (FIFO-like) task chain behavior. This means that when the Dask worker resources are saturated (which is a typical scenario), all submitted tasks have equal priority and are front-loaded to the upstream user-defined PyRosetta protocols, delaying execution of the downstream protocols until all tasks finish the upstream protocols.

2. _Depth-first task execution:_ This PR enables task chains to run to completion, by allowing the user to explicitly increase the priority of tasks submitted to downstream user-defined PyRosetta protocols. This means that when the Dask worker resources are saturated, once a task finishes an upstream protocol, it is submitted to the next downstream protocol with a higher priority than tasks still queued for the upstream protocols, so task chains may run through all protocols to completion.

For example, to run user-defined PyRosetta protocols with depth-first task execution, the `priorities` keyword argument is implemented in this PR where higher priorities take precedence:
```
PyRosettaCluster(...).distribute(
    protocols=[protocol_1, protocol_2],
    priorities=[0, 10],
)
```
Say the user has 10,000 tasks and only 10 Dask worker threads to run on, then with depth-first task execution, the process is as follows:
1. All 10,000 tasks are queued to run `protocol_1`
2. 10 tasks immediately are scheduled to run `protocol_1` on available Dask worker resources
3. As the 10 tasks complete `protocol_1`, they immediately are scheduled to run `protocol_2` before the other 9,990 tasks queued to run `protocol_1` are scheduled
4. As those 10 tasks complete `protocol_2`, they are saved to disk, and the next 10 tasks immediately are scheduled to run `protocol_1`
5. _Etc._

Note that in distributed cluster scenarios, tasks are scheduled on the remote cluster _asynchronously_ from task submissions from the client, so due to normal cluster-specific network latencies, even if a task's priority is higher, there may be short delays in the Dask worker receiving the task, leading to slightly nondeterministic behavior in practice, but in general the task execution pattern follows the user's priority specifications.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `8bb54e8a6dc3`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.