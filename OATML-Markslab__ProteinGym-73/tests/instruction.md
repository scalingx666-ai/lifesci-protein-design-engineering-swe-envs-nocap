# Issue

**Title:** Add SiteRM model

## Description

# Summary

This PR adds the SiteRM model from our NeurIPS 2024 paper [Ultrafast classical phylogenetic method beats large protein language models on variant effect prediction](https://papers.nips.cc/paper_files/paper/2024/hash/eb2f4fb51ac3b8dc4aac9cf71b0e7799-Abstract-Conference.html) . SiteRM is an _independent-sites_ model which relies only on MSA information (like EVMutation) and yet does remarkably well.

SiteRM scores both the DMS zero shot substitutions and the clinical zero shot  substitutions datasets. The results are as follows:

# DMS zero shot  substitutions

![image](https://github.com/user-attachments/assets/3bcafb1b-2734-4891-8b80-488875f66b7e)

# Clinical zero shot  substitutions

![image](https://github.com/user-attachments/assets/90000a4d-31cf-42dd-b738-ad33ff5295a4)

# Reproducing results

To reproduce the results above, just make sure to `pip install cherryml`. Then you can follow the standard pipeline, e.g. for DMS zero shot substitutions:

```
$ cd scripts/scoring_DMS_zero_shot/
$ time bash scoring_SiteRM_substitutions.sh
$ bash merge_all_scores.sh && bash performance_substitutions.sh
$ cd ../..
$ cat benchmarks/DMS_zero_shot/substitutions/AUC/Summary_performance_DMS_substitutions_AUC.csv
```

The model will be trained on the spot when running the script `scoring_SiteRM_substitutions.sh`. It takes ~2 hours to run on my Mac. The number of cores used to parallelize computation can be changed in the script `scoring_SiteRM_substitutions.sh` (the default is 8). The results reproduce to within +-0.001 for all metrics owing to random seed initialization.

Thanks for this great benchmarking resource!

Best,
Sebastian Prillo

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `a81df4bf7129`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.