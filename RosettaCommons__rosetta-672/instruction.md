# Issue

**Title:** Modernize copy prevention in utility/graph EdgeList/Node/Edge to use = delete

## Description

## Summary

Replace old-style private-and-undefined copy constructor/assignment with explicit `= delete` in six internal classes across `Graph.hh` and `Digraph.hh`:

- `Graph.hh`: `EdgeList`, `Node`, `Edge`
- `Digraph.hh`: `DirectedEdgeList`, `DirectedNode`, `DirectedEdge`

Also corrects the copy assignment signatures from the non-standard `T&` argument to the correct `T const &` form. The `Graph` class itself (which has a real, implemented copy constructor and assignment) is unchanged.

## Test plan

- [x] `python3 ./scons.py -j16 mode=debug` passes

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `5c9f6db69d1a`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.