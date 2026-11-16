# Issue

**Title:** Negative unit test for bad patches

## Description

I'm making a unit test out of [my recent post in the debugging channel](https://rosettacommons.slack.com/archives/C1YUVUD5E/p1728433183443519). It may not be the perfect error message but it is better than a seg fault and invites the user to expand upon the error message.

I wrote this code in the browser so I'm expecting a bit of beautification and typo correction to be necessary.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `470f09a88403`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.