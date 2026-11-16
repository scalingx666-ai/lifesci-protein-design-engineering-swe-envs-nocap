# Issue

**Title:** Create eval_motif_benchmark_without_genie2_format.py

## Description

### Add direct PDB motif scaffolding without Genie2 format requirement

Enable (single and multi) motif scaffolding with standard PDB files, eliminating the need for Genie2 format conversion. Preserves all original eval_motif_benchmark.py functionality,

### Single motif scaffolding
```
python eval_motif_benchmark_without_genie2_format.py
--config multimotif_vp
--params params/multimotif_vp-200k.jax
--template heterodimer.pdb
--motif_spec "A:6-82"
--scaffold_length 40
--num_designs 10
```

### Multi-motif scaffolding
```
python eval_motif_benchmark_without_genie2_format.py
--template complex.pdb
--motif_spec "A:20-60,B:15-50"
--scaffold_length 80
```

### Multiple motifs same chain
```
python eval_motif_benchmark_without_genie2_format.py
--template protein.pdb
--motif_spec "A:10-30,A:50-70"
--scaffold_length 60
```

The script automatically falls back to original Genie2 parsing when `motif_spec="none"`, ensuring backward compatibility with existing workflow

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `1253c90d96e3`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.