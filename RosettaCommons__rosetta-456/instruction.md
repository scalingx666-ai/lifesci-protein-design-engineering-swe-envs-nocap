# Issue

**Title:** Add RosettaLigand docking scores to the Pose when JD2 is not active.

## Description

People running RosettaLigand jobs under PyRosetta keep having problems that the computed docking scores don't show up. This is because the current RosettaLigand code was written for RosettaScripts and assumes the use of JD2. The scores get stuck in a JD2 job, which is hard to access when you're not running a command line JD2 job.

We can change the code to stick the scores into the pose instead of (in addition to) the job when not running under JD2 (e.g. PyRosetta) Hopefully this should make the access easier.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `931f7d9046b1`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.