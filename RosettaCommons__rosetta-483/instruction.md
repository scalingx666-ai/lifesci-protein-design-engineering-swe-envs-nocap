# Issue

**Title:** Fix parse_options arguments with local OptionsCollection in ProtocolSettingsMetric.cc

## Description

If using the local `OptionsCollection` in the `ProtocolSettingsMetric` CompositeStringMetric, the `get_script_vars` and `get_user_options` arguments are swapped relative to the `parse_options` signature. These options are both `true` by default, so this bug has likely gone unnoticed, but would only have an effect when `get_script_vars` and `get_user_options` have different values.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `fce88fc4bd52`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.