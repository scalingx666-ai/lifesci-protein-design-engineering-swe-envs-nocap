# Issue

**Title:** Update JCIM citation

## Description

## Summary

- Replace the preprint citation with the published JCIM article in the README.
- Update `CITATION.cff` so GitHub's citation metadata points to the peer-reviewed article.
- Retain the Zenodo DOI as the software archive identifier.

## Why

The CatIF-RL paper is now published in *Journal of Chemical Information and Modeling* (DOI: 10.1021/acs.jcim.6c01587).

## Validation

- Parsed `CITATION.cff` as YAML and verified its preferred-citation DOI, journal, year, and month.
- Ran `git diff --check`.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `915d65f72998`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.