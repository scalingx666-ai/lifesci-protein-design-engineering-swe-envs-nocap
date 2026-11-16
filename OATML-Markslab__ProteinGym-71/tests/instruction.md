# Issue

**Title:** Update to supervised results and scoring scripts

## Description

All supervised substitution results have been updated and now accurately reflect the latest version of ProteinGym. 
- Supervised models have been rerun on all DMS substitution assays for all CV splits. 
- A new script (`merge_supervised.py`) processes all supervised model predictions by computing Spearman correlation and MSE. All results are stored in a single csv-file with the following columns: `DMS_id,model_name,fold_variable_name,Spearman,MSE`
- This file is used as the input to `performance_DMS_supervised_benchmarks.py` which aggregates the performance results as described in the original ProteinGym paper.
- Adding a new supervised model to the benchmark involves evaluating the model across assays and splits, organizing the predictions, and updating `config.json` and `proteingym/constants.json` accordingly.
- Per variant prediction results will shortly be made available, where the predictions for a given CV-scheme, model, and assay are saved in `cv_name/model_name/DMS_id.csv`

Examples of how to process results:
```python
python proteingym/merge_supervised.py \
	--DMS_assays_location path/to/cv/files
	--model_scores_location path/to/cv/predictions \
	--merged_scores_dir . \
	--mutation_type substitutions \
	--dataset DMS

python proteingym/performance_DMS_supervised_benchmarks.py \
    --input_scoring_file merged_scores.csv \
    --output_performance_file_folder outputs \
    --DMS_reference_file_path reference_files/DMS_substitutions.csv \
    --number_assay_reshuffle 10000
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e1d603d28ed8`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.