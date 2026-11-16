# Issue

**Title:** Modernize copy prevention in basic_otstream and TracerImpl

## Description

## Summary

- `basic_otstream`: replace private-undefined copy ctor with explicit `= delete` for both copy ctor and copy assignment in the public section. Adds a one-liner comment explaining the destructor side-effect (`delete this->rdbuf()` causes double-free if two objects share the same buffer).
- `TracerImpl`: move the already-`= delete`d copy ctor from private section to public, and add the missing copy assignment `= delete`.

Part of the ongoing Rule-of-Zero / modern C++ idiom cleanup in `basic/`.

🤖 Generated with [Claude Code](https://claude.com/claude-code)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `5c9f6db69d1a`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.