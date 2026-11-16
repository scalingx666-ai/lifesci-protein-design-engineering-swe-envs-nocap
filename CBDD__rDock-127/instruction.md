# Issue

**Title:** bugfix for unclosed brackets in RbtPharmaSF.cxx

## Description

This PR fixes a bug in rbdock's output SD files when using Pharmacophoric Restraints. The issue involved unclosed parentheses that caused parsing errors, such as "ERROR: End of data field name not found" in rdkit's ForwardSDMolSupplier.

Below is an example of a diff between the output files before and after the fix:
```diff
diff -up out_before.sd out_after.sd
--- out_before.sd       2024-08-31 03:14:55.652910046 +0900
+++ out_after.sd        2024-08-31 03:15:11.559731903 +0900
@@ -183,16 +183,16 @@ libRbt.so (24.08, Buildalpha)
 >  <SCORE.RESTR.PHARMA>
 0
 
->  <SCORE.RESTR.PHARMA.con_1
+>  <SCORE.RESTR.PHARMA.con_1>
 0
 
->  <SCORE.RESTR.PHARMA.opt_1
+>  <SCORE.RESTR.PHARMA.opt_1>
 0
 
->  <SCORE.RESTR.PHARMA.opt_2
+>  <SCORE.RESTR.PHARMA.opt_2>
 0
 
->  <SCORE.RESTR.PHARMA.opt_3
+>  <SCORE.RESTR.PHARMA.opt_3>
 0
 
 >  <SCORE.RESTR.norm>
```

The simple patch implemented in this PR resolved my issue, but I am not sure if it is an appropriate approach. I would be grateful if you could review it.

Thank you.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ac8d04bbf716`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.