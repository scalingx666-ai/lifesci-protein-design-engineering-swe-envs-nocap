# Issue

**Title:** Apply Rule of Zero across utility/ helpers

## Description

## Summary

Remove user-declared destructors and trivial copy ctor/assignment implementations whose bodies are equivalent to the compiler-generated defaults. The implicit special members are correct in every case (no owning raw pointers, no side effects in the removed bodies, virtual destructor inheritance preserved through base classes).

### utility/graph/ — non-owning iterator/element helpers

* `Graph.hh`: `EdgeListElement`, `EdgeListIterator`, `EdgeListConstIterator` — drop user dtor and the `= default` copy ctor / assignment.
* `Digraph.hh`: `DirectedEdgeListElement`, `DirectedEdgeListIterator`, `DirectedEdgeListConstIterator` — drop user dtor and member-wise copy ctor / assignment.
* `ArrayPool.hh` (`Array0`): drop user dtor, copy ctor, and self-assignment-guarded copy assignment.
* `LowMemGraph.hh` (`LowMemGraphBase`): drop empty `override` dtor (`utility::VirtualBase` supplies the virtual destructor).
* `UpperEdgeGraph.hh`: drop the empty `UEEdge` dtor and `~UpperEdgeGraph() override = default;`.

`LowMemNode` / `LowMemEdge` are intentionally left untouched — their explicit destructor declarations anchor a load-bearing comment about deliberately not making the destructor virtual.

### utility/

* `DereferenceIterator.hh` (`DereferenceIterator`): drop empty user dtor.

### utility/options/ — empty overriding destructors

Drop `~XxxOption() override {}` from the eight leaf scalar/vector option classes. The implicit destructor is virtual via inheritance from `Option` (which declares a virtual destructor), so dynamic dispatch is preserved.

* `BooleanOption.hh`, `BooleanVectorOption.hh`
* `FileOption.hh`, `FileVectorOption.hh`
* `IntegerOption.hh`, `IntegerVectorOption.hh`
* `RealOption.hh`, `RealVectorOption.hh`

The remaining option classes with the same pattern (`PathOption`, `StringOption`, `ResidueChainVectorOption`, `ScalarOption`, `ScalarOption_T_`, `VectorOption`, `VectorOption_T_`) can be addressed in a follow-up; the eight here form a coherent leaf-scalar-plus-vector subset.

## Test plan

- [x] `cd source && python3 ./scons.py -j16 mode=debug` — clean (no errors)
- [ ] 90 standard tests CI run

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `80efd178b93d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.