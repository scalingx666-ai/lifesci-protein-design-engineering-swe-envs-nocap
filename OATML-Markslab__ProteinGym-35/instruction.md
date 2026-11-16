# Issue

**Title:** [New Model] MULAN-small

## Description

Dear Pascal,

Thanks for your effort and really useful benchmark. I have added the implementation and all the required scripts for the recently released MULAN-small model. It is a 9M parameters protein language model that scores 0.317 Spearman coefficient on the DMS zero-shot substitutions benchmark.

Please find the following files in the commit:
- Implementation of MULAN model `proteingym/baselines/mulan`. It includes `requirements.txt` file. In order to run the evaluation code, you firstly need to install the mulan module:
```bash
cd proteingym/baselines/mulan
pip install -e .
```
- Modified `config.json` file with the added MULAN model.
- Script for the DMS zero-shot substitutions benchmark evaluation `scripts/scoring_DMS_zero_shot/scoring_MULAN_substitutions.sh`.

Regards,
Daria Frolova

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `495cc3051357`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.