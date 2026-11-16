# Issue

**Title:** Patch: Schema required parameters and version validation

## Description

**Schema and Required Parameter Handling Improvements:**

* Added a `MODE_REQUIRED_PARAMS` mapping in `generate_mode_schemas.py` to explicitly define required parameters for each workflow mode, ensuring mode-specific schemas correctly enforce their requirements.
* Removed hardcoded `required` fields from `nextflow_schema.json` and `schemas/nextflow_schema_custom.json` to allow dynamic handling via the schema generation script.
* Added a `manifest` section in `nextflow.config` to include contributor details, project description, DOI, and version, improving project documentation and discoverability. This enforces Nextflow version >= 24.04 as a requirement at launch

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `bd3f3223d71f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.