# Contributing

This document defines how we work: branching, commits, pull requests, review, verification, changelog, and shipping. These are not suggestions. Every gate here exists because something broke without it.

---

## Table of Contents

- [Branching](#branching)
- [Commit Messages](#commit-messages)
- [Pull Requests](#pull-requests)
- [Code Review](#code-review)
- [Verification](#verification)
- [Breaking Changes](#breaking-changes)
- [Changelog](#changelog)
- [Force Push Policy](#force-push-policy)
- [CI Failures](#ci-failures)
- [Pre-Merge Checklist](#pre-merge-checklist)
- [Anti-Patterns](#anti-patterns)

---

## Branching

One task, one branch. Never work directly on `main`, `master`, `release/*`, `develop`, or `staging`.

Branch names follow the pattern:

```
<type>/<short-description>
```

Examples:

```
feat/jwt-refresh
fix/null-response-handler
refactor/db-connection-pool
docs/api-authentication
```

Branches are deleted after merge. No long-lived feature branches.

---

## Commit Messages

We use [Conventional Commits](https://www.conventionalcommits.org/).

### Format

```
<type>(<scope>): <subject>

<body>

<footer>
```

### Types

| Type | Use For |
|------|---------|
| `feat` | New feature |
| `fix` | Bug fix |
| `docs` | Documentation only |
| `style` | Formatting, whitespace — no logic change |
| `refactor` | Code change with no new feature or fix |
| `perf` | Performance improvement |
| `test` | Adding or correcting tests |
| `chore` | Maintenance, dependency updates |

### Subject Line Rules

- Imperative mood: `add feature`, not `added feature`
- Lowercase after the type prefix
- No trailing period
- Maximum 50 characters
- Describe what, not how

```
# Good
feat(auth): add JWT token refresh
fix(api): handle empty response body
refactor(db): simplify connection pooling

# Bad
feat(auth): Added JWT token refresh.   ← past tense, period
Fix API                                 ← no type, vague
refactor: made database better          ← vague, past tense
```

### Body

Explain *why*, not *what* — the diff shows what. Include body for non-obvious changes, breaking changes, changes with trade-offs, or changes tied to a specific issue. Skip it for self-explanatory fixes, dependency bumps, and style changes.

### Footer

Reference issues with `Closes #N`, `Fixes #N`, or `Relates to #N`.

### Breaking Changes

Use `!` in the type and a `BREAKING CHANGE:` footer:

```
feat(api)!: require authentication for all endpoints

BREAKING CHANGE: All API endpoints now require authentication.
Previously, /health and /version were public.

Migration:
1. No action needed for authenticated clients
2. Service monitors must add auth headers
3. Use /ping for unauthenticated health checks

Closes #890
```

### Squash vs Preserve

Squash when the branch has messy history, fix-typo commits, or represents a single logical change. Preserve when the branch contains multiple distinct stages with useful checkpoint history, or has collaborative authorship.

### Co-Author Attribution

When Claude assists with a commit:

```
Co-Authored-By: Claude <noreply@anthropic.com>
```

---

## Pull Requests

### Title

Same format as commit messages:

```
<type>(<scope>): <description>
```

### Body Template

```markdown
## Summary

[2–3 bullet points describing the change at a high level]

## Changes

[Detailed breakdown of what changed, grouped by concern]

## Testing

[How the changes were verified — commands run, outputs observed, evidence captured]

## Screenshots

[If applicable — UI changes, CLI output, API responses]

## Checklist

- [ ] Tests pass
- [ ] Lint passes
- [ ] Types check
- [ ] Documentation updated
- [ ] Breaking changes noted

## Breaking Changes

[If applicable — what breaks and how to migrate]

Closes #N
```

### Labels

| Change Type | Labels |
|-------------|--------|
| `feat` | `enhancement`, `feature` |
| `fix` | `bug`, `fix` |
| `docs` | `documentation` |
| `refactor` | `refactor`, `tech-debt` |
| `perf` | `performance` |
| Breaking change | `breaking-change` |

### Draft vs Ready

Open as **draft** when CI needs to pass first, you want early feedback, or documentation is incomplete.

Open as **ready** when all checks pass and the PR is ready for immediate review.

---

## Code Review

### Two-Stage Process

Every PR goes through two sequential reviews before merge is offered.

**Stage 1 — Specification review.** Answers: *did it do what was asked?*

- Every acceptance criterion must have explicit line-level evidence
- `TODO`/`FIXME` in required areas = fail
- Scope creep is reported but does not auto-fail; human decides

**Stage 2 — Code review.** Answers: *is it well-written?*

Assumes specification compliance is already confirmed.

### Review Dimensions

| Dimension | What to Check |
|-----------|---------------|
| Style | Matches existing codebase patterns |
| Error handling | All failure paths explicitly handled; errors propagate correctly |
| Edge cases | Null, empty, zero, boundary conditions |
| Security | No injection vectors, no exposed secrets, no unsafe operations |
| Performance | No N+1 queries, no blocking in hot paths |
| Naming | Self-documenting; no single-letter variables outside iterators |
| Complexity | No nesting deeper than 3 levels; functions under 50 lines |
| Testability | Dependencies injectable; no hidden global state |

### The 400 LOC Threshold

Review quality degrades sharply beyond 400 lines of changed code. PRs exceeding this should be flagged and split where possible. Deleted lines and lock files are excluded from the count.

### Security: Attack Surface Tracing

For any change touching authentication, input handling, or data persistence, trace data from entry point to sink:

- **Entry points:** form inputs, query parameters, headers, cookies, file uploads, WebSocket messages, environment variables
- **Dangerous sinks:** database queries, file system operations, template output, command execution, external API calls, log files

At each step: is the data validated? Is it sanitised for the sink type? Can it escape its intended context?

No hardcoded secrets, API keys, passwords, or cryptographic material. These become permanent fixtures in git history.

### Confidence Scoring

Only report issues you are confident about:

| Confidence | Level | Action |
|------------|-------|--------|
| 0–25 | Very low | Do not report |
| 26–50 | Low | Note internally only |
| 51–79 | Medium | Report as Minor |
| 80–89 | High | Report as Important |
| 90–100 | Very high | Report as Critical |

Before reporting, ask: does the framework handle this? Does the type system prevent it? Is there context that justifies it? Would fixing it make the code worse? If yes to any, raise the threshold.

---

## Verification

Changes are not done until they have been seen working. Tests passing is not the same thing as working.

### Automated Quality Checks

Run these in order before any PR is marked ready:

1. **Tests** — exit code 0, count captured, no unexpected skips
2. **Lint** — zero errors (warnings acceptable)
3. **Types** — zero type errors
4. **Build** — succeeds if a build step is configured

Failing any of these is a hard stop. Fix it.

**Flaky tests:** if a test fails then passes on retry, it is flaky. Do not ignore flakiness — it erodes trust in the suite and hides real failures. Fix it or explicitly mark and track it.

### Output Verification

After automated checks pass, see it working:

- **Web app:** start the dev server, confirm key pages render, check for console errors, capture screenshots
- **API:** run the server, curl key endpoints, verify status codes and response shapes, check error formatting
- **CLI:** run `--help`, exercise primary commands, check exit codes and error messages
- **Library:** run example usage from documentation, confirm exported types are correct

Evidence must be captured and included in the PR Testing section. Screenshots, curl outputs, and CLI logs are all valid. "It should work because the types are correct" is not evidence.

### Human Checkpoint

Before a PR is marked ready to merge, a human reviews the verification report. This is not a rubber stamp. The reviewer reads the evidence, asks questions if anything is unclear, and explicitly approves.

### Post-Verification Integrity

If you modify code after verification, re-run verification from scratch. A verified hash that does not match the current state is not a verified state. Do not ship code that has changed since it was verified.

---

## Breaking Changes

### What Counts as Breaking

| Signal | Severity |
|--------|----------|
| Removed public function or class | Critical |
| Removed API endpoint | Critical |
| Changed function signature | Critical |
| Changed return type | Critical |
| Renamed public API | High |
| Changed default value | Medium |
| Changed validation rules | Medium |
| Changed sort order or async behaviour | Medium |

Adding new functions, adding optional parameters, and adding new fields to responses are generally not breaking.

### Required Actions

When shipping a breaking change:

1. Use `!` in the commit type and include a `BREAKING CHANGE:` footer
2. Bump the major version (semver)
3. Write `MIGRATION.md` with before/after examples and step-by-step upgrade instructions
4. Add a `### Breaking Changes` section to the CHANGELOG entry
5. Apply the `breaking-change` label to the PR
6. If this is a public API, notify consumers before the release

---

## Changelog

We follow [Keep a Changelog](https://keepachangelog.com/).

### Format

```markdown
# Changelog

All notable changes to this project will be documented in this file.

## [Unreleased]

## [1.2.0] - 2026-04-27
### Added
- New feature X (#123)

### Changed
- Modified behaviour Y (#124)

### Fixed
- Bug fix Z (#125)
```

### Section Order

Always: Added → Changed → Deprecated → Removed → Fixed → Security.

### Entry Rules

- User-facing description, not implementation detail
- Include PR or issue reference: `(#N)`
- Group by type, not by PR
- Omit empty sections
- Do not include internal refactors, test additions, or dependency updates unless they have a user-facing impact or security relevance

### Unreleased Section

New entries go under `[Unreleased]`. On release, move them to a versioned section with today's date.

### Version Bumps

| Changes | Bump |
|---------|------|
| Breaking changes | MAJOR |
| New features | MINOR |
| Bug fixes only | PATCH |

### Comparison Links

Add at the bottom of the file:

```markdown
[Unreleased]: https://github.com/owner/repo/compare/v1.2.0...HEAD
[1.2.0]: https://github.com/owner/repo/compare/v1.1.0...v1.2.0
```

### Merge Conflicts

Changelog conflicts from parallel PRs are common. Always keep both entries. Do not discard either side.

---

## Force Push Policy

Never force push to `main`, `master`, `release/*`, `develop`, or `staging`.

If you need to clean up history on a feature branch, use squash merge at PR time. If you accidentally committed a secret and need to rewrite history on a protected branch, this is an emergency procedure that must be logged with reason, previous HEAD, and new HEAD.

Safe alternatives to force push:

- To clean up messy commits: use squash merge on the PR
- To amend a pushed commit: add a fixup commit, squash at merge
- To rebase: create a new branch rather than rewriting the pushed one

### Branch Protection

Configure in repo settings for all protected branches:

- Require pull request reviews before merging
- Require status checks to pass
- Require branches to be up to date before merging
- Do not allow force pushes
- Do not allow deletions

---

## CI Failures

### What to Do

| Failure Type | Action |
|--------------|--------|
| Test failure | Investigate and fix. Do not ship. |
| Lint failure | Fix or auto-fix. Do not skip. |
| Build failure | Fix. The code is broken. |
| Type error | Fix. Do not ship. |
| Timeout (infrastructure) | Re-run. May be transient. |
| Flaky test (passes on retry) | Accept with caveat, track, fix soon. |

CI failures caused by infrastructure (runner out of disk, intermittent network) may be skipped with explicit justification recorded. CI failures caused by the code are hard stops.

If you skip CI, record it:

```json
{
  "shipped_with_caveats": true,
  "caveats": ["CI skipped: runner disk space exhausted, code verified locally"]
}
```

---

## Pre-Merge Checklist

Before requesting review or marking a PR ready:

**Verification**
- [ ] All automated checks pass (tests, lint, types, build)
- [ ] Output verified — seen working, not just assumed
- [ ] Evidence captured and included in PR Testing section

**Code integrity**
- [ ] No uncommitted changes
- [ ] Branch is up to date with base

**Documentation**
- [ ] README updated if features changed
- [ ] CHANGELOG entry added under `[Unreleased]`
- [ ] API docs updated if endpoints changed
- [ ] Migration guide written if breaking changes

**Dependencies**
- [ ] No known security vulnerabilities
- [ ] Lock files committed

**Review**
- [ ] No unresolved comments
- [ ] No pending requested changes

---

## Anti-Patterns

The following patterns appear reasonable under pressure. They are not. Each one has caused real production failures.

**"It should work because..."** — Reasoning from first principles instead of evidence. Run it. See it. Capture it.

**"The tests pass so it works."** — Tests cover what they cover. They do not cover runtime behaviour, integration paths, real data, or environment configuration. Passing tests are necessary, not sufficient.

**"I'm confident it works."** — Confidence is not a verification method. Convert it into evidence.

**"The types are clean so it's fine."** — Types catch type errors. They do not catch async timing bugs, external API drift, environment misconfiguration, or race conditions.

**"It worked before."** — Code changes. Dependencies update. Environments drift. Verify it works now.

**"It's a simple change."** — Off-by-one errors are simple. Null checks in the wrong order are simple. Timezone bugs are simple. Verify proportionally, but verify.

**"We can verify later."** — Later does not come. Verify before shipping or mark explicitly as unverified with risk accepted by a human.

**Modifying code after verification.** — Post-verification changes invalidate the verification. Re-verify from scratch.

**Rubber-stamping the human checkpoint.** — If you're approving without reading the evidence, the checkpoint provides no value and you are flying blind.

**Ignoring flaky tests.** — Flaky tests normalise ignoring failures. Real bugs hide behind "it's probably just the flaky one." Fix flaky tests.
