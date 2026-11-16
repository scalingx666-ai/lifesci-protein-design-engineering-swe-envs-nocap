# Issue

**Title:** ci: gate PRs on Actions and bridge to Blossom

## Description

## Description

Lint and pull-request hygiene only. GitLab stays the source of truth for
builds, GPU tests and benchmarks, so nothing here compiles the extension,
pulls LFS objects, or asks for a GPU.

| Workflow | Job | Gate |
| --- | --- | --- |
| `style.yml` | `prek` | `prek run --all-files`, the hooks pinned in `prek.toml` |
| `pr.yml` | `Title` | PR title is a Conventional Commits subject |
| `pr.yml` | `DCO` | every commit the PR adds carries a matching `Signed-off-by` |

The `prek` pin is read out of `requirements-dev.txt` rather than repeated, so CI
cannot drift from what `pip install -e '.[dev]'` gives a contributor. The Title
job runs `prek run commitizen --stage commit-msg` against the title, so
commitizen's version and pattern stay single-sourced too; it strips a leading
`[PROJ-123]` / `[5123456]` first, which commitizen would otherwise reject.

A PR title is attacker-controlled, so it reaches the script through the
environment, never through `${{ }}` interpolation inside a `run:` block.

## Type of change

- [x] Build / CI

## Checklist

- [x] My commits are signed off (DCO): `git commit -s`
- [x] I have read the Contributing Guidelines
- [x] For a new feature or breaking change, an issue was filed and approved first
- [x] I added or updated tests, and they pass locally
- [x] I updated documentation as needed

<!-- Draft: opened to exercise the three jobs on real runners. -->

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `6e7caa01e1d2`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.