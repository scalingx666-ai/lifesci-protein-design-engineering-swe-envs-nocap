# Issue

**Title:** Moving the TestEnvironmentReproducibility unit test, and updating environment export commands

## Description

The aim of this PR is to:

- Update the pixi environment export command to save the `pixi.lock` file, rather than the generic YAML file export which does not pin package versions necessary for environment reproducibility.
-  Add support for the pixi `--manifest-path` flag and the uv `--project` flag, so that `PyRosettaCluster` simulations can be run from outside of the pixi manifest or uv project directories.
- Add support to cache the pixi manifest file and uv `pyproject.toml` file to PyRosettaCluster results, which are necessary to reproduce the pixi/uv project in conjunction with the cached `pixi.lock` file or `requirements.txt` file.
- Sanitize conda channels of username/password credentials instead of stripping out the entire URL from the exported environment file, since PyRosetta no longer requires a username/password for installatioin, making the simulation reproduction lifecycle simpler by obviating the need to reconfigure the conda channel when recreating a virtual environment.
- Update warning/exception messages for pixi, uv, and mamba, instead of just expecting conda.
- Move the `pyrosetta.distributed.cluster.recreate_environment` function to the [RosettaCommons/pyrosetta-extras](https://github.com/RosettaCommons/pyrosetta-extras) GitHub repository. The reason is that we encountered the "chicken or the egg" problem, where the user must install PyRosetta (typically through an environment manager like pixi, uv, conda, or mamba) in order to gain access to the `recreate_environment` function, but then the `recreate_environment` function cannot be run from inside a virtual environment, since inside the function it calls `pixi install ...` and `uv venv ...` which can implement global interpreter locks that hang indefinitely because the outer environment is already activated. Therefore, the code needs to be run from the system python interpreter, which requires the `recreate_environment` function to exist in a separate repository, as it is not functional in the current state.
- Move the `pyrosetta.tests.distributed.cluster.test_reproducibility.TestEnvironmentReproducibility` unit tests (and supporting modules) into the [RosettaCommons/pyrosetta-extras](https://github.com/RosettaCommons/pyrosetta-extras) GitHub repository. These unit tests do not currently run on the Benchmark server (due to its implementation of a pure virtual environment that does not install `conda`, `mamba`, `uv`, or `pixi`) and were originally added to test the `recreate_environment` function from the [RosettaCommons/pyrosetta-extras](https://github.com/RosettaCommons/pyrosetta-extras) GitHub repository (however that presented a second "chicken or the egg" problem which will be solved in a separate PR for that repository).

The following is unrelated, but updated while here:
- Add a more helpful exception when using `LocalCluster(security=True)` and the `cryptography` package is not installed.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `80031f772eb0`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.