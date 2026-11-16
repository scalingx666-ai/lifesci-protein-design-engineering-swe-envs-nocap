# Issue

**Title:** Adding a unique date/time to output directory name

## Description

@nskirk suggested a short function to add to the end of the output directory name that will give a unique time and date:
`pdj_results/${new Date().format('yyyyMMdd_HHmmss')}`

This prevents outputs from being overwritten. I've added it as a suggestion in the comments.

I noticed BindSweeper was not parsing Groovy functions, so I removed the '${projectDir}' prefix from the default out_dir name. It is still used for the seqera profiles, for which BindSweeper is not relevant.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `625273d5d63b`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.