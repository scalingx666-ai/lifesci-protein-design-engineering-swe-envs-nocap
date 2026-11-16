# Issue

**Title:** Add a CPU-only test suite for the GPU-free helpers

## Description

Adds a small, fast, CPU-only test suite — `biopython_utils.py` currently has no tests. These are **characterization tests** locking the current behavior of the pure helpers; no design/algorithm behavior changes.

**Covers** (19 tests, seconds on a laptop): `set_range`, `calculate_percentages`, `validate_design_sequence`, `calculate_clash_score`, `hotspot_residues`, `target_pdb_rmsd`. `conftest.py` builds tiny synthetic PDB fixtures and reuses `examples/templates/1qys1.pdb` + `examples/targets/pd-l1-1.pdb`.

The `set_range` tests **document (with `BUG:` notes) that its upper bound is exclusive** — `"39-45"` → `39..44`, dropping the last residue of every hotspot/mask window. A follow-up PR fixes it.

Adds `pytest.ini`, `requirements-dev.txt` (CPU-only), and `.venv`/pytest-cache `.gitignore` entries.

```
19 passed
```

**This is the foundation PR** — the set_range / cmap / BindCraft PRs build on it; merge this first.

🤖 Generated with [Claude Code](https://claude.com/claude-code)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `9a7a0941b11e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.