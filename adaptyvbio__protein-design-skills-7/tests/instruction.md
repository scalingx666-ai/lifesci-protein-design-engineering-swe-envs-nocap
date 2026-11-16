# Issue

**Title:** Fix plugin install scoping and stale asset SVGs

## Description

## Why
A fresh-install test surfaced two issues.

**Granular installs did not scope.** Installing `evaluation@` or `boltzgen@` loaded all 24 skills, not the advertised subset. Confirmed against the Claude Code plugin spec: a plugin's skills are discovered by scanning its `source` directory's `skills/`, and the marketplace `skills:` array is a path override, not a whitelist. Every entry used `source: "./"`, so all 24 loaded. Skills cannot be subset per plugin in a flat repo.

**Stale asset SVGs.** The README's `skill_categories.svg` still said "21 skills" / 10 design tools, and other assets had "21 skills", an outdated version, and a wrong install command (`all@` instead of `adaptyv@`).

## What changed
- Marketplace now ships a single `adaptyv` plugin (all 24 skills). Removed the 5 category and 24 single-skill entries that could not scope. `docs/skills.md` and `getting-started.md` updated to a single install.
- SVGs corrected: counts 21 to 24, design-tools to 13, version to v2.1.0, install command to `adaptyv@`.

## Verification
Fresh install in an isolated `CLAUDE_CONFIG_DIR`: marketplace adds, `adaptyv@protein-design-skills` installs and loads exactly 24 skills, all frontmatter parses, all internal links resolve. Removed granular plugins now correctly 404. All SVGs are well-formed XML.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `655d83a624a7`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.