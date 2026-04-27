# Contributing

Contributions are welcome. This document describes the full process expected of every contributor.

---

## Code Quality

Only submit code you have reviewed line by line, understand fully, and can defend. Naive or default implementations are not acceptable. If you cannot explain a decision, it should not be in the PR.

---

## Workflow

Every non-trivial change follows this sequence:

1. **Open an issue first.** Agree on scope and approach before writing code. Surprises in PRs get closed.
2. **One branch per unit of work.** Do not bundle unrelated changes.
3. **Open a PR.** Fill in the template fully — summary, evidence, testing, checklist.
4. **Pass review.** Two review passes required before a human merge decision.
5. **Human approves.** No auto-merge, ever.

---

## Branches

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

Never push directly to `main`, `master`, or any `release/*` branch. Force push to protected branches is not permitted under any circumstance.

---

## Commit Messages

Follow [Conventional Commits](https://www.conventionalcommits.org/):

```
<type>(<scope>): <subject>

[body]

[footer]
```

**Types:** `feat` · `fix` · `docs` · `style` · `refactor` · `perf` · `test` · `chore`

**Subject line:**
- Imperative mood — `add feature`, not `added feature`
- Lowercase after the type
- No trailing period
- 50 characters max
- What changed, not how

**Body:** Explain *why*, not what. Include when the reason is non-obvious, trade-offs exist, or the change addresses a specific subtle issue.

**Breaking changes:** Append `!` to the type and add a `BREAKING CHANGE:` footer:

```
feat(api)!: require authentication on all endpoints

BREAKING CHANGE: All endpoints now require a valid Authorization header.
Previously /health was unauthenticated.

See MIGRATION.md for upgrade steps.
```

---

## Before Opening a PR

Passing tests alone is not sufficient. You must observe actual output:

- Run the thing. See it work.
- Capture evidence: CLI output, API responses, screenshots, logs.
- Include that evidence in the PR Testing section.

If you cannot show it working, the PR will not be reviewed.

---

## Review

Every PR goes through two review passes:

1. **Spec review** — does the implementation match what was agreed in the issue? Must pass.
2. **Code review** — correctness, security surface, quality. Must pass, or concerns must be explicitly acknowledged before merge.

Both reviews must complete before a human merge decision is made.

---

## CHANGELOG

Every PR that changes behaviour must include an entry under `[Unreleased]`:

```markdown
## [Unreleased]

### Added
- What was added (#PR)

### Changed
- What changed (#PR)

### Fixed
- What was fixed (#PR)

### Breaking Changes
- What broke and how to migrate (#PR)
```

---

## What Gets Merged

- Code that solves a real problem, clearly.
- Implementations that are correct, not just functional.
- PRs with observable evidence that the change works.
- Changes that respect existing architecture and conventions.

## What Does Not Get Merged

- Half-finished work.
- Implementations the submitter cannot explain.
- PRs without evidence.
- Anything that skips the review process.
- Changes made after review passed.

---

Maintainer: [@elb-pr](https://github.com/elb-pr)
