# Governance

## Maintainer

This project is maintained by [@elb-pr](https://github.com/elb-pr). All decisions are made by the maintainer.

## Branch Strategy

- `main` — production‑ready code only. Direct pushes are restricted.
- `feat/<description>` — new features
- `fix/<description>` — bug fixes
- `chore/<description>` — maintenance, dependencies, tooling
- `docs/<description>` — documentation only
- `refactor/<description>` — code restructuring without behaviour change

Task branches are created one‑per‑task by `claudikins-kernel:execute`. One task = one branch. Agents never run `git checkout`, `git merge`, or `git push` to protected branches directly — those operations are owned by kernel commands.

## Merge Strategy

- Feature and fix branches: **squash merge** into `main`
- Commit message must follow Conventional Commits: `type(scope): description`
- No merge commits on `main`
- Branches deleted after merge

## Versioning

This project follows [Semantic Versioning](https://semver.org/):

| Change | Bump |
|--------|------|
| Breaking change | MAJOR |
| New feature | MINOR |
| Bug fix | PATCH |

## Releases

- Tagged from `main` only
- Every release has a corresponding `CHANGELOG.md` entry
- Breaking changes noted explicitly in release notes

---

## Two‑Stage Review

Every task branch goes through two reviewers before merge. They check orthogonal dimensions.

| Reviewer | Model | Question | Focus |
|----------|-------|----------|-------|
| spec‑reviewer | haiku | "Did it do what was asked?" | Requirement compliance, acceptance criteria coverage, scope creep |
| code‑reviewer | opus | "Is it well-written?" | Quality, security, maintainability, edge cases |

**Neither reviewer can substitute for the other.** Both verdicts are required before a merge is offered.

### 400 LOC Threshold

Review efficacy degrades rapidly above 400 lines of changed code. Before spawning reviewers, the diff is estimated:

| LOC | Quality | Action |
|-----|---------|--------|
| 0–200 | Excellent | Proceed |
| 200–400 | Good | Proceed |
| 400–600 | Degraded | Flag for human decision |
| 600+ | Poor | Must split before review |

Deleted lines and lock files are excluded from the count.

---

## RESOLVE: Conflict Resolution Framework

When spec‑reviewer and code‑reviewer disagree, conflicts are categorised by type.

### Type I — Stylistic (Subjective)

Examples: naming style, indentation, import ordering.

Resolution: defer to the style guide; if silent, author's preference prevails. Mark as `Nit:` (non‑blocking). These should be caught by linters, not humans.

### Type II — Empirical (Fact‑Based)

Examples: performance claims, security vulnerabilities, correctness bugs.

Resolution: **data wins**. The reviewer asserting a problem must provide evidence — a benchmark, a complexity analysis, a concrete exploit path. If evidence supports the claim, the code must change.

### Type III — Architectural (Design)

Examples: design pattern choice, library selection, modularity boundaries.

Resolution: synchronous discussion first; document the trade-off in the PR; if unresolved, escalate to the maintainer for a binding decision.

---

## Verdict Matrix

| Spec | Code | Action |
|------|------|--------|
| PASS | PASS | Accept |
| PASS | CONCERNS (minor) | Accept with notes |
| PASS | CONCERNS (important) | Human decision: accept caveats / fix / escalate |
| PASS | CONCERNS (critical) | Must fix — no exceptions for security issues |
| FAIL | PASS | Revise implementation or update spec |
| FAIL | CONCERNS | Revise implementation |
| FAIL | FAIL | Major revision: retry task or escalate |

### Decision Tree

```
Spec verdict?
├── PASS →
│   Code verdict?
│   ├── PASS → Accept
│   ├── CONCERNS (minor) → Accept with notes
│   ├── CONCERNS (important) → Spec gap? Yes → follow-up task / No → fix first
│   └── CONCERNS (critical) → Security? Yes → must fix / No → human decides
└── FAIL →
    Reason?
    ├── Missing requirement → Revise implementation
    ├── Scope violation → Revert additions, retry
    └── Wrong approach → escalate or replan
```

---

## Confidence Scoring

Code‑reviewer only reports issues it is confident about.

| Confidence | Level | Action |
|------------|-------|--------|
| 0–25 | Very low | Do not report |
| 26–50 | Low | Note internally only |
| 51–79 | Medium | Report as Minor |
| 80–89 | High | Report as Important |
| 90–100 | Very high | Report as Critical |

---

## Escalation Paths

### Escalate to maintainer when:

- Reviewers directly contradict each other
- Failure reason is unclear or not actionable
- Two fix attempts have failed review
- Spec may itself be wrong

### Accept with caveats when:

- Spec did not require X and code does not have X (known limitation)
- Minor style issues that do not affect correctness
- Performance trade-off that is documented
- Tech debt noted for a follow-up task

---

## Anti‑Patterns

- **"Spec passed, ship it."** — Code concerns may reveal spec gaps. Review both before accepting.
- **"Opus is smarter, trust code‑reviewer."** — Reviewers check different things. Both matter.
- **Auto‑resolving conflicts algorithmically** — Humans must review conflicts and make informed decisions.
- **Ignoring the 400 LOC threshold** — Large diffs produce degraded reviews. Split tasks before review, not after.
