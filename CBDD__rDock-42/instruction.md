# Issue

**Title:** Changed creation of target directories during installation

## Description

Encountered the following error during installation in an ubuntu-22.04 machine:

```
$ PREFIX=/data1/SOFT/test make install
 [...]
 cp: target '/data1/SOFT/test/bin' is not a directory
 make: *** [Makefile:119: install] Error 1
```

The resulting failed installation creates an empty folder named '{bin,lib,include}'. It corrects by changing line [Makefile#L167](https://github.com/CBDD/rDock/blob/eb979e0aac26ed8b8127e390063c2adfbcd2553a/Makefile#L167) in my setup.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `eb979e0aac26`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.