# Issue

**Title:** Set correct scope for automatic protocol detection in PyRosettaCluster reproduction

## Description

If using the `pyrosetta.distributed.cluster.reproduce` function and a list of user-defined PyRosetta protocols is not provided, then it attempts to automatically detect the protocols from the current stack frame. However, the function is naively searching in `globals()` from `tools.py`, and thus only searches for the protocols in the global variables of `tools.py` and not from the user's script. This PR fixes the automatic detection by instead searching up two frames to the caller of the `pyrosetta.distributed.cluster.reproduce` function. 

Furthermore, this PR extends functionality to search for protocols in both `globals()` and `locals()` of the user's script. In this way, whether the user imports/defines their protocols in the global or local variables, the protocols are detected.

Example of detecting protocols in global variables:
```
from pyrosetta.distributed.cluster import reproduce
from my_protocols import protocols # Protocols are in global variables

def main():
    reproduce(...)

if __name__ == "__main__":
    main()
```

Example of detecting protocols in local variables:
```
from pyrosetta.distributed.cluster import reproduce

def main():
    from my_protocols import protocols # Protocols are in local variables

    reproduce(...)

if __name__ == "__main__":
    main()
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `fce88fc4bd52`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.