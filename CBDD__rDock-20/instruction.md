# Issue

**Title:** add github workflow for multiple distro builds

## Description

with this PR, we're creating a github actions workflow that will run on every PR or merge to main branch.
It builds rDock from scratch, installing all required dependencies within containers for ubuntu 22.04, ubuntu 20.04, debian 10, debian 11, and centos stream8.

In the future, I'll add
* caching for the base build images
* an official image with rDock installed
* a .deb/.rpm package for supported distributions
* support for windows and osx

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `562ee9581fa1`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.