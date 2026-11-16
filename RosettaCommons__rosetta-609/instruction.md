# Issue

**Title:** Updated PARCS applications and IMMS_CCS score function

## Description

This application builds on the existing PARCS (parcs_ccs_calc.cc) application and the IMMS_CCS energy term originally developed by smturzo.

I extended PARCS to support multimeric protein complexes, enabling simulation of PARCS CCS data for input structures containing multiple chains. In addition, the existing IMMS_CCS energy term, which was previously limited to monomers, was generalized for complexes through the introduction of a new IMMS_ComplexCCS_Energy term. I also implemented a new CCS_IMMS_with_CryoEMEnergy score term that integrates experimental CCS data with cryo-EM information.

Method: For PARCS multimer support, I introduced a boolean flag (-multimer) to the existing PARCS application. When enabled, the algorithm predicts CCS values for multimeric assemblies by reparameterizing the original CCS calculation.
For the IMMS-based energy terms, I developed on the existing CCS_IMMSEnergy implementation (source/src/core/energy_methods/CCS_IMMSEnergy.cc/.hh) by adding new energy classes:
CCS_IMMSComplexEnergy, which enables CCS-based scoring for protein complexes
CCS_IMMS_with_CryoEMEnergy, which incorporates cryo-EM restraints alongside experimental CCS data

Integration test: I did integration just like how it was done for monomers with additional flag. for multimer The test passed.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `307045a3f7fe`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.