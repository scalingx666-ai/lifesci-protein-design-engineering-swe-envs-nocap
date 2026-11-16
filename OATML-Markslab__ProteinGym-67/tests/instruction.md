# Issue

**Title:** [New Model] RSALOR

## Description

Dear ProteinGym Team,

We would like to submit our model, [RSALOR](https://www.biorxiv.org/content/10.1101/2025.02.03.636212v1), for inclusion in the ProteinGym benchmark repository under the "DMS zero-shot" category.
The model is implemented as a simple pip package, and its source code is available in this [GitHub repository](https://github.com/3BioCompBio/RSALOR).

As required, we have included the necessary scripts for replicating the results:
- Scoring script: `./proteingym/baselines/RSALOR/run_rsalor.py`
- Example bash script: `./scripts/scoring_DMS_zero_shot/scoring_RSALOR_substitutions.sh`

Additionally, we have added relevant metadata for the model in:
- `./config.json`
- `./proteingym/constants.json`

We appreciate your time and consideration. Please let us know if any modifications are needed.
All the best,
Matsvei 🙂

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e1d603d28ed8`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.