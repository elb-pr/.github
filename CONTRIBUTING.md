# Contributing

Contributions are welcome. This document describes the full process expected of every contributor whether human or agent.

## Code Quality Standard

Code you write, understand fully, and can defend line by line.

Acceptable if you have reviewed every line, understand what it does, it is not a naive or default implementation, and you can defend every decision if asked.

## Branch Naming

Task branches:

```
task-<N>-<slug>
```

Human branches:

```
feat/<description>
fix/<description>
chore/<description>
docs/<description>
refactor/<description>
perf/<description>
test/<description>
style/<description>
```

Never push directly to `main`, `master`, or `release`. Force push to protected branches is forbidden.

## Commit Messages

```
<type>(<scope>): <subject>

<body>

<footer>
```

### Types

| Type | Use for |
|---|---|
| `feat` | New feature |
| `fix` | Bug fix |
| `docs` | Documentation only |
| `style` | Formatting, no logic change |
| `refactor` | Code change, no new feature or fix |
| `perf` | Performance improvement |
| `test` | Adding or updating tests |
| `chore` | Maintenance, dependencies, tooling |

### Subject line rules

- Imperative mood — `add feature` not `added feature`
- Lowercase after the type
- No trailing period
- Max 50 characters
- What changed, not how

### Body

Explain **why**, not what. Include a body when the reason is non-obvious, there are trade-offs, or the change fixes a specific non-obvious issue.

### Breaking changes

Append `!` to the type and include a `BREAKING CHANGE:` block in the footer:

```
feat(api)!: require authentication on all endpoints

BREAKING CHANGE: All endpoints now require authentication.
Previously /health was public.

Migration: add Authorization header to all requests.
See MIGRATION.md for full steps.
```

## Verification Evidence

Passing tests is not sufficient evidence that an implementation works.

Before opening a PR:

- Observe actual output: CLI output, API responses, rendered UI, screenshots
- Evidence goes in the PR Testing section — commands run, outputs observed, screenshots
- The verified commit SHA and file manifest must match what is in the PR

## Review Gates

Every PR requires two automated review passes before a human merge decision:

1. **Spec compliance** — verifies the implementation matches what was agreed. Must return PASS.
2. **Code quality** — checks correctness, security surface, quality. Must return PASS or CONCERNS explicitly acknowledged by a human.

No PR merges without human approval. Auto-merge is disabled.

## CHANGELOG

Every PR that changes behaviour must include a CHANGELOG entry under `[Unreleased]`:

```markdown
## [Unreleased]

### Added
- Description of new thing (#PR)

### Changed
- Description of change (#PR)

### Fixed
- Description of fix (#PR)

### Breaking Changes
- Description of break and migration path (#PR)
```

## What Gets Merged

- Code that solves a real problem, clearly
- Implementations that are correct, not just functional
- PRs with observable evidence that the implementation works
- Changes that respect existing architecture and conventions
- Full compliance with the contributing document

## What Does Not Get Merged

Half-finished work, implementations the submitter cannot explain, PRs without verification evidence.

Maintainer: [@elb-pr](https://github.com/elb-pr)

