# Issue

**Title:** macOS support

## Description

Dear rDock members, we are developing an unofficial rDock plugin for `Horus` at the Barcelona Supercomputing Center. You can learn more about `Horus` [in the official webpage](horus.bsc.es).

`Horus` natively supports Linux and macOS and we wanted to distribute the plugin for both platforms. In order to do so, I had to compile rDock on macOS manually. For it to work I had to slightly modify some parts of the code and the makefiles. 

During my tests, I did not see any change in the results outputs and the application is working flawlessly. I hope that this macOS support allows rDock to run on more platforms!

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `9b4272039b38`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.