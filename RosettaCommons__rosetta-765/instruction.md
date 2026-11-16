# Issue

**Title:** Restore CA_only default for AddConstraintsToCurrentConformationMover in RosettaScripts

## Description

### Problem
The [documentation](https://docs.rosettacommons.org/docs/latest/scripting_documentation/RosettaScripts/Movers/movers_pages/AddConstraintsToCurrentConformationMover) and the C++ default (`atom_sele_ = AtomSelector::CA_ONLY` in the header) both say that `CA_only` defaults to true. But a tag with none of `CA_only` / `bb_only` / `sc_tip_only` set constrains **every heavy atom**:

```xml
<AddConstraintsToCurrentConformationMover name="csts"/>  <!-- documented: CA only; actual: all heavy atoms -->
```

This is a regression from the `sc_tip_only` change (PR #5074 in the old repository, merge commit `ed038c21cd`). Before that change, `parse_my_tag` read `CA_only_ = tag->getOption< bool >( "CA_only", CA_only_ )` with `CA_only_( true )`. The refactor to the `AtomSelector` enum starts `parse_my_tag` from a local `AtomSelector sele = AtomSelector::ALL` and always calls `set_atom_selector( sele )`, which replaces the CA-only default.

### Fix
After the existing three option blocks, fall back to `CA_ONLY` unless `CA_only="false"` or another subset was requested:

| XML | Before this PR | After |
|---|---|---|
| *(none)* | ALL | **CA_ONLY** (as documented, and as before `ed038c21cd`) |
| `CA_only="false"` | ALL | ALL |
| `bb_only="true"` | BB_ONLY | BB_ONLY |
| `sc_tip_only="true"` | SC_TIP_ONLY | SC_TIP_ONLY |
| two or more set to true | error | error |

The XSD description of `CA_only` now states the default, and says how to get all heavy atoms.

### Tests
Built and run locally (macOS arm64, clang 17, `mode=release`):
- **Unit:** new `test_parse_atom_selector` covers the cases above through `parse_my_tag`. The existing tests only build the mover in C++, which is why none of them caught this. `AddConstraintsToCurrentConformationMoverTests` passes with the fix. Without the fix, the new test fails on the no-option case only.
- **rosetta_scripts:** on the integration test's `obj01.pdb`, `<AddConstraintsToCurrentConformationMover/>` now adds 2 CA coordinate constraints instead of 22 heavy-atom ones. `CA_only="true"` (2), `CA_only="false"` (22) and `bb_only="true"` (10) are unchanged.
- **Integration:** `AddConstraintsToCurrentConformationMover` output is unchanged (byte-identical before and after). That test doesn't record the constraints: its `flags` raise the level of the old `protocols.simple_moves.AddConstraintsToCurrentConformationMover` tracer, but the mover now logs under `protocols.constraint_movers.…`. I've left that as is to keep this PR focused.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `369e400415dd`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.