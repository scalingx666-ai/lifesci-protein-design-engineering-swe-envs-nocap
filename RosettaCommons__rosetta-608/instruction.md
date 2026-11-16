# Issue

**Title:** Minor updates to uv project export machinery in PyRosettaCluster

## Description

This minor PR addresses:
- Instead of relying on an exported `requirements.txt` file from the uv project, the `uv.lock` file is cached (similar to Pixi which caches the `pixi.lock` file). The reason for this change is several-fold:
  - The `requirements.txt` file does not pin the Python version being used to execute the `PyRosettaCluster` simulation. We were relying on the `pyproject.toml` file to provide this information, however, if the user doesn't pin the Python version in it, then there's no record of which Python version is being used (other than inspecting the PyRosetta build string or another hack).
  - The `uv export` command doesn't provide sha256 hashes for custom registries. Now that `pyrosetta` is shipped quarterly, this disallows using the `uv pip sync --require-hashes` flag which would match hashes for each requirement, making reproduction more robust. While `uv pip compile` does support the `--generate-hashes` flag, again the Python version is not captured in a `requirements.txt` file.
  - Currently, `uv export` doesn't support emitting the `--find-links` paths into the exported `requirements.txt` file. While `uv pip compile` does support the `--emit-find-links` flag, again the Python version is not captured in a `requirements.txt` file.
- Ensures the `self.toml_format` attribute is still set for the edge case where the uv `pyproject.toml` file cannot be automatically located via either the `UV_PROJECT` environment variable or the current working directory (which currently leads to an `AttributeError` upon saving results). A more helpful warning message is provided herein.
- Disables two `FutureWarning`s regarding the `filter_results` and `norm_task_options` class attributes being enabled by default, which were introduced in version `2.1.0` (merged ~8/25) and `3.0.0` (merged ~10/25), respectively.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `1a56185c2592`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.