# Issue

**Title:** Modernize copy prevention in Emitter, YamlEmitter, and JsonEmitter

## Description

## Summary

- `Emitter`, `YamlEmitter`, `JsonEmitter`: replace private/protected undefined copy ctor declarations with explicit `= delete` in the public section, and add the missing copy assignment `= delete`.
- Also remove the redundant undefined null-ctor declarations in `YamlEmitter` and `JsonEmitter` — these were suppressed automatically in C++11 by the presence of user-declared non-default constructors.

Part of the ongoing Rule-of-Zero / modern C++ idiom cleanup in `basic/`.

🤖 Generated with [Claude Code](https://claude.com/claude-code)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `5c9f6db69d1a`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.