# Issue

**Title:** Fix loading of ligands when three letter code matches NCAA

## Description

@LAnAlchemist noticed that when a ligand params file provided with `-extra_res_fa` has a three letter code which matches an NCAA three letter code from the database, that ligand ResidueType is never selected on PDB read-in, even if it's a much better match for the names in the PDB.

The reason for this is that the PDB reader residue typer prioritizes patched polymeric terminus types (those with TERMINUS properties) for residues at the ends of chains, discarding the ligand types as a possibility before even encountering the name-based selection.

This PR adjusts how the typer selects residues. Instead of having chain-terminal residues preferring terminus properties, actually look at the connection points, and look for residues which have/don't have the UPPER & LOWER connection points. (This is really what "is_lower_terminus" and "is_upper_terminus" in PoseFromSFRBuilder signifies: is this residue polymerically connected to the adjacent residue.)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `fce88fc4bd52`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.