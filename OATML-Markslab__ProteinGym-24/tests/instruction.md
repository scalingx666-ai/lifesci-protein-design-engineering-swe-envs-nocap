# Issue

**Title:** [New Model] SaProt implementation

## Description

Dear Pascal,

I have uploaded the implementation of SaProt, including both 650M and 35M version. The result csv file is also contained in the ``baselines/saprot`` for check.

To reproduce the result, please download our checkpoints [here](https://huggingface.co/westlake-repl) and foldseek binary file to create 3Di sequence. And then run the ``scoring_SaProt_substitutions.sh``.

If you have any question, feel free to email me for further discussion :)

Best regards,
Jin

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `495cc3051357`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.