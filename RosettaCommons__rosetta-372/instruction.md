# Issue

**Title:** Add option to control intermediate dumping in BackrubProtocol

## Description

Especially with XML (where the Backrub protocol is part of a more extensive protocol),  you don't necessarily want an unconditional dumping of the last & low results from the Backrub stage.  This PR adds an XML option which will allow the dumping of the poses (old behavior), but turns it off by default.  This only changes XML usage -- I didn't change how the backrub application behaves.

 Changes to the backrub_interface_ddG integration test is expected.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `74f819c4dd58`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.