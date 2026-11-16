# Issue

**Title:** Fix issue related to PackerTask usage in homodimer_design.

## Description

With a -pack_min_runs of 2 or greater, there's a chance that the sequence of the structure changes. This is a problem for a fixed PackerTask run, as `task_is_valid( pose )` fails if the base residue types don't match up properly. This is an issue found in Discussion #112 

The fix is simple -- just use the TaskFactory which we already have lying around instead of manually generating the packer task. (The PackRotamersMover will properly re-apply the TaskFactory.) We can also make the interface of PackRotamersMover a bit nicer

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `f5ae1de8e146`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.