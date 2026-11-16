# Issue

**Title:** Installation and running fixes

## Description

Dear developer(s),

thanks for this great software! However, it took some effort to get it working.
This PR fixes 2 issues:
- 8ced19b fixes an installation issue I encountered when I build the container inside rootless docker (or with singularity for that matter). A similar issue (the uid/gid where different on my build) and the solution are described [here](https://superuser.com/a/1435438)
- 07d5a46 Is a more elaborate fix that solves the error you get when you tried to run the code with the `--no-pyrosetta` flag from a different working directory than the main code directory, which results in the openmm code throwing `ModuleNotFoundError: No module named 'functions'`. This was an issue for both a bare metal installation on our test system as for a container based run, as you needed to know the name of the output directory instead of just mounting a user directory inside the container and run from there. Also on our HPC, we can only use singularity, no docker, which mounts and runs from the current working directory by default.

If you want I can split the PR up, or you can cherry pick-over the individual commits. Please let me know if you need any more info!

PS: if anyone else ever need to build a singularity image from this repo, converting the DockerFile to a singularity recipe with [singularity python](https://singularityhub.github.io/singularity-cli/recipes#auto-detection) works reasonably well (after the fix in 8ced19b), but if you want to install pyrosetta you need to hardcode that in the recipe instead of feeding it from the command line

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e4f3852304a4`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.