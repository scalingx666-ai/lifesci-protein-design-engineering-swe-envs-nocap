# Issue

**Title:** Supporting a universal PyRosetta initialization file

## Description

This PR aims to add support for a PyRosetta initialization file type (i.e., a `.init` file) for reproducible PyRosetta initialization. 

What started out as a quick and dirty method to cache `.params` files morphed into a universal file type for streamlining PyRosetta initialization. This approach takes advantage of the `ProtocolSettingsMetric` SimpleMetric in PyRosetta to cache Rosetta command line options and parser script variables, and `zlib`, `base64`, and `json` libraries for compressing/decompressing arbitrary input text and binary files, including files within files (i.e., subfiles). Subfiles are cached by doing a brute force search through all input files by splitting file contents on spaces, which enables, for example, caching conformer files on `PDB_ROTAMERS` lines within `.params` files, and caching subfiles in list files passed in with the `-l` Rosetta command line flag. Note that the PyRosetta database and any input directories are not cached in the `.init` file.

The following new methods are supported in this PR:

- `pyrosetta.dump_init_file`: write a PyRosetta initialization `.init` file
- `pyrosetta.init_from_file`: initialize PyRosetta from a `.init` file
- `pyrosetta.get_init_options`: get the currently initialized Rosetta command line options
- `pyrosetta.get_init_options_from_file`: get PyRosetta initialization options from a `.init` file

This is a work in progress and suggestions/recommendations are welcome. Once merged, ideally the `.init` file format can remain stable over incremental PyRosetta versions.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `078f4d967cba`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.