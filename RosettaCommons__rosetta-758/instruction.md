# Issue

**Title:** Fix Segfault with backrub.

## Description

While BackrubProtocol clears the BackrubMover's segments between proteins, the bond_angle_map_ data set by add_segment() is not.

This means that when you apply backrub to a large protein and then to a small protein, the residue numbers from the large protein cause a segmentation fault in optimize_branch_angles() when the residual data in bond_angle_map_ is applied to the small protein.

Clearing bond_angle_map_ when we clear the segments addresses the issue.

Thanks to use @tla268 from Discussion #756 for the use case which demonstrated the issue.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `de92a3c0dea8`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.