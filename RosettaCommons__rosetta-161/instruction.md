# Issue

**Title:** Remove is_surface() from Residue/ResidueType.

## Description

This function is confusing, as it's not talking about the surface of a protein, but mineral surfaces.

The ResidueType property SURFACE isn't used frequently -- for the rare case it's needed, we can use the more involved query protocol, rather than leaving the potentially confusing `Residue::is_surface()` lying around.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `200d5f9a7d8c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.