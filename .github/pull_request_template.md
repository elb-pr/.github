<!--
  Title format : type(scope): short description
  Types        : feat | fix | docs | style | refactor | perf | test | chore
  Scopes       : auth | api | ui | db | config | deps | hooks | agents | skills | commands
  Rules        : imperative mood · lowercase · no period · max 50 chars
  Breaking     : feat(scope)!: description  +  BREAKING CHANGE: footer in body
  Example      : feat(auth): add JWT token refresh
-->

## Summary

<!--
  2–3 bullets. What changed and why — not how.
  Focus on user/system impact.
-->

-
-

## Changes

**New**

-

**Modified**

-

**Deleted**

-

## Verification Evidence

<!--
  `claudikins-kernel:verify` MUST have passed before this PR opens.
  "Tests pass" is not evidence. Show it working.
  Paste screenshots, curl responses, or CLI output below.
-->

- [ ] `claudikins-kernel:verify` passed — all phases PASS
- [ ] Verified commit SHA matches `verify-state.json → verified_commit_sha`
- [ ] File manifest hash matches `verify-state.json → verified_manifest`
- [ ] Code has NOT been modified since verification ran

**Evidence:**

```

```

## Testing

- [ ] Test suite passes
- [ ] Lint — 0 issues
- [ ] Type check — 0 errors
- [ ] Build succeeds
- [ ] Output verified by catastrophiser (screenshot / curl / CLI)
- [ ] Any flaky failures re-run in isolation and documented

## Documentation

- [ ] README updated if features, usage, or installation changed
- [ ] CHANGELOG entry added under `[Unreleased]` (Keep a Changelog format)
- [ ] Version bumped in manifest file if applicable
- [ ] `MIGRATION.md` written if this is a breaking change

## Breaking Changes

<!--
  Delete this section if no breaking changes.
  If breaking: document what breaks and the full migration path.
  Requires: BREAKING CHANGE footer in commit · MAJOR version bump · MIGRATION.md.
-->

**What breaks:**

**Migration path:**

- [ ] `BREAKING CHANGE:` footer in commit message
- [ ] CHANGELOG `### Breaking Changes` section written
- [ ] MAJOR version bump applied
- [ ] `MIGRATION.md` created or updated

## Review Gates

- [ ] Spec reviewer verdict: **PASS**
- [ ] Code reviewer verdict: **PASS** (or CONCERNS explicitly acknowledged below)
- [ ] Human has reviewed and approved — no auto-merge

<!--
  If code reviewer returned CONCERNS, document the acknowledged concerns here:
-->

## CI

- [ ] All CI checks pass
- [ ] No force push to `main` / `master` / `release`

<!--
  If a check failed or was skipped, document the explicit accepted caveat here.
  "CI is flaky" is not a caveat.
-->

```

```

Closes #
