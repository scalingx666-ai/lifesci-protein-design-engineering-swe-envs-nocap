# Issue

**Title:** fix boltz filter typo

## Description

Minor typo fix for boltz filtering:

```
filter_boltz.py: error: unrecognized arguments: --boltz-min-ptm-interface 
```

"_" delimited nextflow param gets sanitized to "-" but python script expects mixed delimiter: --boltz-min-ptm_interface

Modified python script to expect "-" delimited argument.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `72c63bce9f35`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.