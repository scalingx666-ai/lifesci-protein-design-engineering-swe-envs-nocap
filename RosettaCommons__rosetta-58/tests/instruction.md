# Issue

**Title:** Fix pre-compile script calls on Windows build

## Description

The windows build test on the test server doesn't seem to be updating options appropriately. 

It looks like the issue is that the *.sh files aren't being properly called -- adding "bash" to the front of them allows them to run properly:

Before:
```
Running versioning script ... Done. (0.4 seconds)
```

After:
```
Running versioning script ... Done. (0.4 seconds)
file ./options.dox being updated
file ./full-options-list.md being updated
Number of option files updated: 2
Total 4680 options.
Updating submodules needed for compilation.
Finished updating ResidueProperty code
-- no changes needed
Finished updating VariantType code
-- no changes needed
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `baad48240e60`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.