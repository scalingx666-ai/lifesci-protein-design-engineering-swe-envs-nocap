# Issue

**Title:** [New Model] ESCOTT

## Description

Hi ProteinGym team! 

We would like to contribute to the zero-shot substitution benchmark with our model [ESCOTT](http://gitlab.lcqb.upmc.fr/tekpinar/PRESCOTT/), presented in [PRESCOTT: a population aware, epistatic and structural model accurately predicts missense effect](https://www.medrxiv.org/content/10.1101/2024.02.03.24302219v1.full), an updated version of GEMME that introduces structural information in the model.

ESCOTT achieves a weighted average Spearman of 0.473 (computed with `scripts/scoring_DMS_zero_shot/performance_subsitutions.sh`) and a simple average of 0.497. Results for the single assays can be found on [Zenodo](https://zenodo.org/records/13946943).

Due to the many dependencies inherited by GEMME and the additional required installation of DSSP program, we provide a dedicated Docker image with the properly configured environment. It can be pulled from Docker Hub with
```bash 
sudo docker pull tekpinar/prescott-docker:v1.6.0
```
To reproduce the results, run `scripts/scoring_DMS_zero_shot/scoring_ESCOTT_substitutions.sh`.  
The script executes `proteingym/baselines/ESCOTT/compute_fitness.py` inside the Docker container, with the mounted data volumes. To run the script without `root` permissions, the user has to be added to `docker` group.

Please let us know if anything is missing, and if the provided solution with Docker matches your requirements.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `3ba691dc221e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.