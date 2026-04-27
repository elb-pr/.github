# Governance

## Maintainer

This project is maintained by [@elb-pr](https://github.com/elb-pr). All decisions are made by the maintainer.

## Branch Strategy

- `main` — production-ready code only. Direct pushes are restricted.
- `feat/<description>` — new features
- `fix/<description>` — bug fixes
- `chore/<description>` — maintenance, dependencies, tooling
- `docs/<description>` — documentation only
- `refactor/<description>` — code restructuring without behaviour change

## Merge Strategy

- Feature and fix branches: **squash merge** into `main`
- Commit message must follow [Conventional Commits](https://www.conventionalcommits.org/): `type(scope): description`
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
