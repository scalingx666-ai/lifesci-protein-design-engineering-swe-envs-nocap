# Issue

**Title:** Test and fix for mp_f19_decoy_discrimination failure

## Description

The mp_f19_decoy_discrimination scientific test has a hard failure. It's crashing with the following issue in the minimization routine of relax. 
```
core.scoring.hbonds.hbonds_geom: [ ERROR ] NAN occurred in H-bonding calculations!
core.scoring.hbonds.hbonds_geom: [ ERROR ] Dxyz                    -NAN                    -NAN                    -NAN  Hxyz                    -NAN                    -NAN                    -NAN
```

This PR pulls out the offending run/structure as an integration test. 

Playing around with it, I figured out two things
1. The `-constrain_relax_to_start_coords true` flag is needed to show symptoms
2. It was specifically the derivative calculations which were causing NaN issues.

Inserting a number of `std::isnan()` checks and trying to trace back the source indicates the problem was `MembraneGeometry::g_radius_gradient_dz()`, specifically where it did a `(p.x()-xo)/std::abs(p.x()-xo))`, which results in a NaN if p.x() equals xo.  We can rearrange this slightly, not only to avoid the NaN, but also to reduce the number of calculations needed. While I was at it, additional efficiency changes in MembraneGeometry were also made.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `80e656e7195c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.