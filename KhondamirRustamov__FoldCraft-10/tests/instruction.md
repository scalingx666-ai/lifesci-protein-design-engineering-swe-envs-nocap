# Issue

**Title:** Fix set_range off-by-one: make hotspot/mask ranges inclusive

## Description

## What

`set_range` parses hotspot/mask range strings like `"30-34"`. The current
implementation is **upper-bound exclusive** — `"30-34"` expands to
`{30, 31, 32, 33}`, silently dropping residue 34. This narrows every hotspot and
mask window by one residue. Since these ranges decide which residues the
fold-conditioned cmap loss conditions on, it's accuracy-relevant. This PR makes
the upper bound **inclusive**: `"30-34"` → `{30, 31, 32, 33, 34}`.

## This matches the cmaps shipped in this repo

The `examples/cmaps/*.npy` added in 9a7a094 are the canonical conditioned contact
maps. I reconstructed the inter-chain hotspot block of each one purely from the
hotspot strings in the matching `examples/scripts/design_*.sh`, and compared
exclusive vs inclusive parsing against the published arrays:

| Fold | script target_hotspots | published cmap active target residues | parse that matches |
|------|------------------------|----------------------------------------|--------------------|
| Top7 (1qys) | `30-34,50-54,69-76` | 30,31,32,33,**34**, 50,51,52,53,**54**, 69…**76** | **inclusive** |
| α-solenoid (3jx8) | `30-34,50-54,69-76` | …,**34**, …,**54**, …,**76** | **inclusive** |
| Ig-like (3sd2) | `30-34,50-54,69-76` | …,**34**, …,**54**, …,**76** | **inclusive** |
| Ankyrin (5aao) | `30-34,50-54,69-76` | …,**34**, …,**54**, …,**76** | **inclusive** |
| TIM (5bvl) | `29-35,49-55,68-77` | …,**35**, …,**55**, …,**77** | **inclusive** |
| β-barrel (6d0t) | `30-34,50-54,69-76` | …,**34**, …,**54**, …,**76** | **inclusive** |

All six folds, **both** the target and binder hotspot blocks, match **inclusive**
parsing. The binder-side blocks tell the same story (e.g. 6d0t binder
`12-15,25-31,40-43` → published cmap includes 15, 31, 43).

So the published reference cmaps were built with inclusive ranges, but today's
`set_range` (exclusive) builds the cmap at runtime one residue short in every
window — meaning `FoldCraft.py` run against the example scripts does **not**
reproduce the `.npy` files shipped alongside them. This PR resolves that
inconsistency.

## Tests

Adds adversarial cases locking the inclusive semantics: `"39-45"` → `39..45`,
`"80-81"` → `[80, 81]`, `"80-80"` → `[80]`, mixed ranges + singletons, and order
preservation.

> Note: because this shifts every hotspot/mask window by one residue, design
> success rates move per-config. That's expected — the point is correctness
> against the shipped reference cmaps, not a tuning change.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `9a7a0941b11e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.