# Contributing

Contributions are welcome. This document describes the full process expected of every contributor — human or agent.

---

## Code Quality Standard

**Human-written:** Code you wrote, understand fully, and can defend line by line.

**AI-assisted:** Acceptable if you have reviewed every line, understand what it does, it is not a naive or default implementation, and you can defend every decision if asked. Vibe-coded submissions — generated and submitted without genuine review — will be closed without merge.

This is not about whether AI was used. It is about whether the person submitting understands and stands behind what they are submitting.

---

## The Contribution Flow

Every non-trivial change follows this sequence. No shortcuts.

```
outline → execute → verify → ship
```

1. **Outline** — Open an issue or discussion first. Agree on approach before writing code. For agents: produce a plan using `claudikins-kernel:outline`. Plan format defines tasks; tasks become branches.
2. **Execute** — One task = one branch. Branches are created via `claudikins-kernel:execute`. Two-stage review (spec reviewer then code reviewer) runs before any merge is offered.
3. **Verify** — Run `claudikins-kernel:verify`. All phases must PASS. A `verify-state.json` is produced containing the verified commit SHA and file manifest hash. This is the gate.
4. **Ship** — Run `claudikins-kernel:ship`. The ship command enforces that verify ran, that code has not changed since verification, and that a human approves the final merge. No auto-merging.

---

## Branch Naming

Task branches (agent-created):

```
execute/task-<N>-<slug>     e.g. execute/task-3-add-jwt-refresh
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

---

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

Explain **why**, not what. The diff shows what. Include a body when the reason is non-obvious, there are trade-offs, or the change fixes a specific non-obvious issue. Skip for self-explanatory changes, dependency updates, and formatting fixes.

### Breaking changes

Append `!` to the type and include a `BREAKING CHANGE:` block in the footer:

```
feat(api)!: require authentication on all endpoints

BREAKING CHANGE: All endpoints now require authentication.
Previously /health was public.

Migration: add Authorization header to all requests.
See MIGRATION.md for full steps.
```

Breaking changes also require: MAJOR version bump, `### Breaking Changes` section in CHANGELOG, and a `MIGRATION.md`.

### Issue references

```
Closes #123
Fixes #456
Relates to #789
```

### AI attribution

```
Co-Authored-By: Claude <noreply@anthropic.com>
```

---

## Verification Evidence

Passing tests is not sufficient evidence that an implementation works.

Before opening a PR:

- Run `claudikins-kernel:verify` — all phases (tests, lint, types, build, output verification) must PASS
- Observe actual output: CLI output, API responses, rendered UI, screenshots
- Evidence goes in the PR Testing section — commands run, outputs observed, screenshots
- The verified commit SHA and file manifest must match what is in the PR. Any change after verification means re-running verify

Submissions that rely solely on test results as proof will be asked for evidence before merge.

---

## Review Gates

Every PR requires two automated review passes before a human merge decision:

1. **Spec reviewer** — verifies the implementation matches what was agreed. Must return PASS.
2. **Code reviewer** — checks correctness, security surface, quality. Must return PASS or CONCERNS explicitly acknowledged by a human.

No PR merges without human approval. Auto-merge is disabled.

---

## CHANGELOG

Every PR that changes behaviour must include a CHANGELOG entry under `[Unreleased]` following [Keep a Changelog](https://keepachangelog.com) format:

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

Do not leave the changelog for later. It is part of shipping, not an afterthought.

---

## What Gets Merged

- Code that solves a real problem, clearly
- Implementations that are correct, not just functional
- PRs with observable evidence that the implementation works
- Changes that respect existing architecture and conventions
- Full compliance with the `outline → execute → verify → ship` flow

**What does not get merged:** half-finished work, implementations the submitter cannot explain, PRs without verification evidence, anything that skips the verify gate, or changes made after verify passed.
