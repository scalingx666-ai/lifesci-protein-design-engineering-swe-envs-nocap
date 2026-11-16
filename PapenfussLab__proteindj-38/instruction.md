# Issue

**Title:** Fix MacOS incompatible xargs flag

## Description

A user reported that the download_models.sh script failed on macosx due to osx xargs not having a -d option (#37)

Changed xargs -d '\n' → tr '\n' '\0' | xargs -0:
- xargs -d '\n' is a GNU extension unavailable on macOS BSD xargs
- xargs -0 (null-delimited) is portable across both platforms
- tr '\n' '\0' converts newline-separated filenames from grep into null-terminated ones that xargs -0 expects

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `7db6ac1da0cc`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.