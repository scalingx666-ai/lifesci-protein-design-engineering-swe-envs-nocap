# Issue

**Title:** Support yielding PackedPose objects in memory from PyRosettaCluster

## Description

The aim of this PR is to provide support for a new `PyRosettaCluster.generate` method that may be used for developing PyRosetta protocols on a local or remote compute cluster and optionally post-processing or visualizing output `PackedPose` objects in memory. 

Normally, the `PyRosettaCluster.distributed` method is used as a job distributor that writes results to disk, however it can useful to access results directly from a `PyRosettaCluster` simulation in memory, and view them in molecular visualization software like `py3Dmol` in a Jupyter notebook, while developing input PyRosetta protocols. For example, a user-defined PyRosetta protocol may involve GPU computation, and so rather than solely developing protocols on an unequipped head node, protocols can be developed over a remote compute cluster and results analyzed on-the-fly, before launching large-scale simulations using the `PyRosettaCluster.distributed` method. 

Importantly, any subsequent code run on the yielded results is not captured by `PyRosettaCluster`, and so use of the `PyRosettaCluster.generate` method does not ensure reproducibility of the simulation. Therefore, a salient warning has been added about this notable difference in methods. Use of the `PyRosettaCluster.distribute` method is for reproducible simulations, and use of the `PyRosettaCluster.generate` method is for protocol development and debugging (and it can be used in any disposable simulations that do not require reproducibility). In this way, this PR adds some flexibility for using `PyRosettaCluster`.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c07e51cae6c8`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.