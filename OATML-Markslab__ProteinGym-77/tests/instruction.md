# Issue

**Title:** Add AIDO.Protein-RAG-proteingym-dms-zeroshot

## Description

I made the following changes
* `proteingym/baselines/AIDO.Protein-RAG-proteingym-dms-zeroshot`
* `scripts/scoring_DMS_zero_shot/scoring_AIDO.Protein-RAG-proteingym-dms-zeroshot.sh`

Model can be found at [https://huggingface.co/genbio-ai/AIDO.Protein-RAG-16B-proteingym-dms-zeroshot](https://huggingface.co/genbio-ai/AIDO.Protein-RAG-16B-proteingym-dms-zeroshot). Data (MSA/Structure) can be found at [https://huggingface.co/datasets/genbio-ai/ProteinGYM-DMS-RAG-zeroshot](https://huggingface.co/datasets/genbio-ai/ProteinGYM-DMS-RAG-zeroshot).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `1f8de974dead`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.