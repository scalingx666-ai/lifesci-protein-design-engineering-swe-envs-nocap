# Issue

**Title:** Various fixes

## Description

Changelist:

`train.py`
- Updated zero-shot column names to match ProteinGym
- Added try/except statement for loading checkpoints
- Changed `os.mkdir`to `os.makedirs(..., exist_ok=True)` to avoid parallel creations leading to crashes
 
`proteinnpt/baselines/model.py` 
- Comment out `ConvBertLayer` functionality (no longer supported by `transformers`)

`proteinnpt/proteinnpt/model.py`
- Comment out `ConvBertLayer` functionality (no longer supported by `transformers`)
- Fixed OHE dimension bug

`proteinnpt/utils.data_utils.py`
- Now reconstructs wild type sequence from mutated sequence and adds as argument to `cleanup_ids_assay_data()` to avoid errors

`proteinnpt/baselines/model_configs/OHE_DeepSequence_final.json`
- Fixed augmentation key to align with others and avoid errors

`proteinnpt/utils/data_utils.py`
- Remove wild type row from zero-shot dataframe to avoid errors (e.g. for DeepSequence)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `a51858bc33a3`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.