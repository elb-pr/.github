# Contributing

Contributions are welcome. This document explains what is expected.

## Code Quality Standard

All contributed code must meet one of the following criteria:

**Human-written:** Code you wrote yourself, understand fully, and can explain line by line.

**AI-assisted, high quality:** Code produced with AI assistance is acceptable if:
- You have reviewed every line and understand what it does
- It is not a naive or default implementation — it solves the problem well
- You can defend every decision if asked
- It does not introduce unnecessary complexity, cargo-culted patterns, or placeholder logic dressed as a real implementation

Vibe-coded submissions — code generated and submitted without genuine understanding or review — will be closed without merge.

This is not about whether AI was used. It is about whether the person submitting understands and stands behind what they are submitting.

## Implementation Standard

Passing tests is not sufficient evidence that an implementation works. Before submitting:

- Run your code and observe actual output — CLI output, API responses, rendered UI
- If tests pass but you have not seen the thing work end-to-end, it is not ready
- Submissions that rely solely on test results as proof of correctness will be asked to provide evidence before merge

Include that evidence in the Testing section of the PR template: commands run, outputs observed, screenshots where applicable.

## Process

1. Open an issue first if the change is non-trivial — agree on the approach before writing code
2. Fork the repository and create a branch using the naming convention below
3. Write your changes
4. Open a pull request using the provided template — fill it out completely
5. Be prepared to discuss the implementation

## Branch Naming

```
feat/<description>      new feature
fix/<description>       bug fix
chore/<description>     maintenance, dependencies, tooling
docs/<description>      documentation only
refactor/<description>  restructuring without behaviour change
perf/<description>      performance improvement
test/<description>      tests only
style/<description>     formatting, no logic change
```

## Commit Messages

Format:

```
<type>(<scope>): <subject>

<body>

<footer>
```

### Types

| Type | Use for |
|------|---------|
| `feat` | New feature |
| `fix` | Bug fix |
| `docs` | Documentation only |
| `style` | Formatting, no code change |
| `refactor` | Code change, no new feature or fix |
| `perf` | Performance improvement |
| `test` | Adding or updating tests |
| `chore` | Maintenance, dependencies |

### Subject line rules

- Imperative mood — "add feature" not "added feature"
- Lowercase after the type — `feat(auth): add JWT middleware` not `feat(auth): Add JWT middleware`
- No trailing period
- Max 50 characters
- Describe what changed, not how

### Body

Explain **why** the change was made. The diff shows what changed. The body explains the reasoning, trade-offs, or context that the diff cannot.

Include a body when:
- The reason for the change is not obvious
- There are trade-offs worth noting
- The change fixes a specific non-obvious issue

Skip the body for self-explanatory changes, dependency updates, and formatting fixes.

### Footer

Reference issues:

```
Closes #123       closes the issue when merged
Fixes #456        for bug fixes
Relates to #789   related but does not close
```

### Breaking changes

Append `!` to the type and include a `BREAKING CHANGE:` block in the footer with a migration path:

```
feat(api)!: require authentication for all endpoints

BREAKING CHANGE: All API endpoints now require authentication.
Previously, /health and /version were public.

Migration:
1. No action needed for authenticated clients
2. Service monitors must add auth headers
3. Use /ping for unauthenticated health checks
```

### AI attribution

If AI tooling assisted in writing the code, include co-author attribution in the commit:

```
Co-Authored-By: Claude <noreply@anthropic.com>
```

## What Gets Merged

- Code that solves a real problem clearly
- Implementations that are correct, not just functional
- Changes that respect the existing architecture and conventions of the project
- PRs with evidence that the implementation actually works

What does not get merged: half-finished work, low-effort ports of existing solutions, implementations the submitter cannot explain, or PRs without observable evidence of correctness.
