# Issue

**Title:** Drop template-id from ctor/dtor names in numeric/ (GCC 14/15 -Werror=template-id-cdtor)

## Description

## Summary

GCC 14+ enforces `-Werror=template-id-cdtor`: a class template may not name its own constructors or destructors with explicit template arguments — the injected-class-name must be used without `<...>`. `numeric/MathVector.hh`, `numeric/MathMatrix.hh`, and `numeric/histograms/OneDHistogram.hh` declared constructors (and, for the first two, the destructor) in the disallowed form, e.g.:

```cpp
MathVector< T>() : ...
explicit MathVector< T>( const Size SIZE, ... ) : ...
~MathVector< T>() { ... }
OneDHistogram<key1>()= default;
```

Under GCC 14/15 in C++20 mode this fails to compile (`error: template-id not allowed for constructor/destructor in C++20`). The `numeric/` occurrences broke the plain library debug build before it could even reach `basic/` or `core/`. The `OneDHistogram.hh` occurrence is more subtle: its default constructor is only instantiated by a unit test (`numeric/histograms/OneDHistogram.cxxtest.hh`), not by any library code, so it slipped past a library-only build and only surfaced when building/running the unit test suite — this is what broke CI on the previous version of this PR.

This drops the `< T>` / `<key1>` from the constructor and destructor declarator-ids, making them consistent with the copy constructors in the same classes, which already use the correct injected-class-name form. Return types, operators, and `new` expressions that legitimately use the templated name are untouched. Pure syntactic correction, no semantic change.

Verified with a clean `mode=debug` library build **and** `mode=debug cat=test` unit-test build under GCC 15 (this machine's default), plus a full library build under GCC 14 — both fully green. A targeted scan of `source/src`, `source/test`, and `source/src/devel` for the same declaration pattern turned up no other occurrences.

## Relationship to existing GCC 15 PRs (#555, #556)

This is not the first attempt at the GCC 15 build. Two earlier community PRs are still open and overlap with this one:

- **#555 — "Fix C++20 template-id errors in constructors/destructors"** (@saberger). This contains the **identical** MathVector/MathMatrix/OneDHistogram template-id fix as this PR, and additionally covers `core/scoring/lkball/LK_DomeEnergy.cc`, `protocols/forge/remodel/RemodelGlobalFrame.cc`, `utility/options/VectorOption_T_.hh`, two unit tests, and `tools/build/basic.settings` — none of which have this same template-id-cdtor pattern currently (checked directly), so those are addressing separate GCC 15 issues.
- **#556 — "Fixes for compiling with gcc15"** (@roccomoretti). The broader assorted GCC 15 fixes. Does not touch these three files.

Because #555 already lands the same fix, #723 is largely redundant with the `numeric/` slice of that effort. It's offered as a minimal, narrowly-scoped version of just the `template-id-cdtor` correction in case it's useful to merge the build-blocking part independently; otherwise #555 (combined with #556, per its author's note) supersedes it. Closing this in favor of #555 + #556 is fine if the maintainers prefer to consolidate the GCC 15 work.

## Note on diff size

This is a small change (three files, ~18 lines) — below the usual bundling threshold — but it is the complete fix for this pattern across the codebase: a scan of `source/src`, `source/test`, and `source/src/devel` found no other occurrences of a class template naming its own constructor/destructor with a template-id. Kept narrowly scoped to this one toolchain-compatibility pattern.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c0fbd1cd0e78`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.