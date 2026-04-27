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

## Process

1. Open an issue first if the change is non-trivial — agree on the approach before writing code
2. Fork the repository and create a branch following the naming convention: `feat/`, `fix/`, `chore/`, `docs/`, `refactor/`
3. Write your changes
4. Open a pull request using the provided template — fill it out properly
5. Be prepared to discuss the implementation

## Commit Messages

Follow [Conventional Commits](https://www.conventionalcommits.org/):

```
type(scope): description

feat(auth): add JWT middleware
fix(api): handle null response in user endpoint
docs(readme): update installation instructions
```

Types: `feat`, `fix`, `chore`, `docs`, `refactor`, `perf`

Breaking changes: append `!` to the type — `feat!:` — and describe the migration path in the PR.

## What Gets Merged

- Code that solves a real problem clearly
- Implementations that are correct, not just functional
- Changes that respect the existing architecture and conventions of the project

What does not get merged: half-finished work, low-effort ports of existing solutions, or anything the submitter cannot explain.
